# Redis 内存数据库 -- 量化交易的实时数据中枢

## 概述

Redis 是一个开源的内存数据结构存储系统，由 Salvatore Sanfilippo 于 2009 年创建。它支持字符串、哈希、列表、集合、有序集合等多种数据结构，并提供发布/订阅、事务、Lua 脚本、流等高级功能。在量化交易领域，Redis 是实时行情缓存、交易信号传递和分布式锁的核心基础设施。其微秒级的响应延迟和高吞吐量使得它成为低延迟交易系统中不可或缺的组件。

## 基本信息

| 项目 | 详情 |
|------|------|
| 名称 | Redis (Remote Dictionary Server) |
| 开发者 | Salvatore Sanfilippo / Redis Inc. |
| 首次发布 | 2009年 |
| 当前版本 | 7.2+ |
| 许可证 | BSD 3-Clause |
| 编程语言 | C |
| 官网 | https://redis.io |
| GitHub | https://github.com/redis/redis |
| 适用平台 | Linux / macOS / Windows |

## 核心特性

### 1. 实时行情缓存

```python
import redis
import json
import time
from datetime import datetime

class MarketDataCache:
    '''实时行情缓存管理器

    使用Redis缓存实时行情数据，
    支持多策略共享同一数据源。
    '''
    def __init__(self, host='localhost', port=6379, db=0):
        '''初始化Redis连接

        Args:
            host: Redis主机地址
            port: Redis端口
            db: 数据库编号
        '''
        self.redis = redis.Redis(
            host=host, port=port, db=db,
            decode_responses=True
        )

    def update_price(self, symbol, price, volume, timestamp=None):
        '''更新实时价格

        使用Redis Hash存储最新行情，同时维护价格历史。

        Args:
            symbol: 交易标的
            price: 最新价格
            volume: 最新成交量
            timestamp: 时间戳
        '''
        if timestamp is None:
            timestamp = time.time()

        # 存储最新行情 (Hash)
        key = f'quote:{symbol}'
        self.redis.hset(key, mapping={
            'price': str(price),
            'volume': str(volume),
            'timestamp': str(timestamp),
            'updated': datetime.utcnow().isoformat()
        })

        # 维护价格时间序列 (Sorted Set, score=timestamp)
        ts_key = f'price_ts:{symbol}'
        self.redis.zadd(ts_key, {str(price): timestamp})

        # 只保留最近1000个价格点
        self.redis.zremrangebyrank(ts_key, 0, -1001)

        # 设置过期时间
        self.redis.expire(key, 3600)  # 1小时过期
        self.redis.expire(ts_key, 86400)  # 24小时过期

    def get_latest_price(self, symbol):
        '''获取最新价格

        Args:
            symbol: 交易标的

        Returns:
            价格信息字典
        '''
        key = f'quote:{symbol}'
        data = self.redis.hgetall(key)
        if data:
            return {
                'symbol': symbol,
                'price': float(data.get('price', 0)),
                'volume': float(data.get('volume', 0)),
                'timestamp': float(data.get('timestamp', 0))
            }
        return None

    def get_price_history(self, symbol, start_time=None, end_time=None):
        '''获取价格历史

        Args:
            symbol: 交易标的
            start_time: 起始时间
            end_time: 结束时间

        Returns:
            价格列表 [(timestamp, price), ...]
        '''
        ts_key = f'price_ts:{symbol}'
        if start_time and end_time:
            data = self.redis.zrangebyscore(ts_key, start_time, end_time)
        else:
            data = self.redis.zrange(ts_key, -100, -1, withscores=True)

        return [(float(score), float(value)) for value, score in data]

    def watch_symbols(self, symbols, callback):
        '''监控多个标的的价格变化

        使用Redis Pub/Sub实现实时价格推送。

        Args:
            symbols: 标的列表
            callback: 价格变化回调函数
        '''
        pubsub = self.redis.pubsub()
        channels = [f'price_update:{s}' for s in symbols]
        pubsub.subscribe(*channels)

        for message in pubsub.listen():
            if message['type'] == 'message':
                data = json.loads(message['data'])
                callback(data)
```

### 2. Pub/Sub 信号传递

```python
import redis
import json
import threading

class TradingSignalBus:
    '''交易信号总线

    使用Redis Pub/Sub在策略进程之间传递交易信号。
    '''
    def __init__(self, host='localhost', port=6379):
        '''初始化信号总线

        Args:
            host: Redis主机
            port: Redis端口
        '''
        self.redis = redis.Redis(host=host, port=port)
        self.subscribers = {}

    def publish_signal(self, channel, signal):
        '''发布交易信号

        Args:
            channel: 信号频道
            signal: 信号数据字典
        '''
        signal['timestamp'] = time.time()
        self.redis.publish(channel, json.dumps(signal))

    def subscribe_signals(self, channel, callback):
        '''订阅交易信号

        Args:
            channel: 信号频道
            callback: 信号处理回调
        '''
        pubsub = self.redis.pubsub()
        pubsub.subscribe(channel)

        def listener():
            for message in pubsub.listen():
                if message['type'] == 'message':
                    signal = json.loads(message['data'])
                    callback(signal)

        thread = threading.Thread(target=listener, daemon=True)
        thread.start()
        self.subscribers[channel] = thread
        return thread

    def publish_trade_signal(self, strategy_name, symbol,
                             action, price, confidence):
        '''发布结构化交易信号

        Args:
            strategy_name: 策略名称
            symbol: 交易标的
            action: 买入/卖出/持有
            price: 建议价格
            confidence: 信号置信度
        '''
        signal = {
            'strategy': strategy_name,
            'symbol': symbol,
            'action': action,
            'price': price,
            'confidence': confidence
        }
        self.publish_signal('trade_signals', signal)


class SignalAggregator:
    '''信号聚合器

    聚合多个策略的交易信号，执行综合决策。
    '''
    def __init__(self, redis_host='localhost'):
        self.bus = TradingSignalBus(redis_host)
        self.signals = {}
        self.lock = threading.Lock()

    def on_signal_received(self, signal):
        '''处理接收到的信号

        Args:
            signal: 交易信号
        '''
        symbol = signal['symbol']
        with self.lock:
            if symbol not in self.signals:
                self.signals[symbol] = []
            self.signals[symbol].append(signal)

            # 聚合信号
            all_signals = self.signals[symbol]
            buy_count = sum(1 for s in all_signals if s['action'] == 'buy')
            sell_count = sum(1 for s in all_signals if s['action'] == 'sell')

            # 多策略共识决策
            if buy_count >= 2:
                print(f'共识买入信号: {symbol}, '
                      f'策略数: {buy_count}/{len(all_signals)}')
            elif sell_count >= 2:
                print(f'共识卖出信号: {symbol}, '
                      f'策略数: {sell_count}/{len(all_signals)}')

    def start(self):
        '''启动信号聚合器'''
        self.bus.subscribe_signals('trade_signals', self.on_signal_received)
        print('信号聚合器已启动')
```

### 3. 分布式锁

```python
import redis
import uuid
import time

class DistributedLock:
    '''分布式锁实现

    使用Redis实现跨进程/跨机器的互斥锁，
    确保关键操作的原子性。
    '''
    def __init__(self, redis_client, lock_name, ttl=10):
        '''初始化分布式锁

        Args:
            redis_client: Redis客户端
            lock_name: 锁名称
            ttl: 锁超时时间(秒)
        '''
        self.redis = redis_client
        self.lock_name = f'lock:{lock_name}'
        self.ttl = ttl
        self.identifier = str(uuid.uuid4())

    def acquire(self, timeout=5):
        '''获取锁

        Args:
            timeout: 获取锁的超时时间(秒)

        Returns:
            是否成功获取锁
        '''
        end_time = time.time() + timeout
        while time.time() < end_time:
            # SET NX EX 原子操作
            acquired = self.redis.set(
                self.lock_name, self.identifier,
                nx=True, ex=self.ttl
            )
            if acquired:
                return True
            time.sleep(0.05)
        return False

    def release(self):
        '''释放锁

        使用Lua脚本确保只释放自己持有的锁。
        '''
        lua_script = '''
        if redis.call("get", KEYS[1]) == ARGV[1] then
            return redis.call("del", KEYS[1])
        else
            return 0
        end
        '''
        result = self.redis.eval(
            lua_script, 1, self.lock_name, self.identifier
        )
        return result == 1

    def __enter__(self):
        '''上下文管理器支持'''
        self.acquire()
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        '''自动释放锁'''
        self.release()


class OrderExecutionGuard:
    '''订单执行保护器

    使用分布式锁防止重复下单。
    '''
    def __init__(self, redis_client):
        self.redis = redis_client

    def execute_order_safely(self, symbol, action, quantity,
                             order_func):
        '''安全执行订单

        Args:
            symbol: 交易标的
            action: 买入/卖出
            quantity: 数量
            order_func: 实际下单函数

        Returns:
            下单结果
        '''
        lock_name = f'order:{symbol}:{action}'
        lock = DistributedLock(self.redis, lock_name, ttl=5)

        if lock.acquire(timeout=3):
            try:
                # 检查是否已有相同订单
                order_key = f'pending_order:{symbol}'
                existing = self.redis.get(order_key)

                if existing:
                    print(f'已有待执行订单: {symbol}, 跳过')
                    return None

                # 执行下单
                result = order_func(symbol, action, quantity)

                # 记录待执行订单
                self.redis.setex(order_key, 60, json.dumps({
                    'symbol': symbol,
                    'action': action,
                    'quantity': quantity
                }))

                return result
            finally:
                lock.release()
        else:
            print(f'获取锁失败: {lock_name}')
            return None
```

## 应用场景

### 实时行情缓存
- 多策略共享行情数据源
- 减少重复数据获取
- 微秒级数据访问

### 交易信号传递
- 策略间信号广播
- 多策略共识决策
- 信号聚合与过滤

### 分布式锁
- 防止重复下单
- 资源互斥访问
- 关键操作原子性保证

### 会话管理
- 交易会话状态存储
- 用户认证Token管理
- 限流和配额控制

## 优缺点分析

### 优点
- **极低延迟**：内存操作，微秒级响应
- **数据结构丰富**：String、Hash、List、Set、ZSet、Stream
- **原子操作**：支持事务和Lua脚本
- **持久化**：支持 RDB 和 AOF 两种持久化方式
- **高可用**：支持主从复制和哨兵模式
- **Pub/Sub**：内置发布/订阅功能

### 缺点
- **内存成本**：全内存存储，数据量大时成本高
- **数据集限制**：受限于物理内存大小
- **不支持复杂查询**：不支持 SQL 式查询
- **集群复杂**：Redis Cluster 配置和管理较复杂
- **Windows 支持弱**：官方不再维护 Windows 版本

## 与其他工具对比

| 特性 | Redis | Memcached | Hazelcast |
|------|-------|-----------|-----------|
| 数据结构 | 丰富 | 简单 | 丰富 |
| 持久化 | 支持 | 不支持 | 支持 |
| Pub/Sub | 支持 | 不支持 | 支持 |
| 分布式锁 | 支持 | 不支持 | 支持 |
| 集群 | 支持 | 客户端分片 | 支持 |
| 内存效率 | 高 | 极高 | 中 |

## 推荐资源

### 官方资源
- Redis 文档：https://redis.io/documentation
- Redis 命令参考：https://redis.io/commands
- Python 客户端：https://github.com/redis/redis-py

### 扩展工具
- **Redis Stack**：包含搜索、JSON、时序等模块
- **RedisInsight**：可视化管理工具
- **Celery**：基于 Redis 的任务队列

## 总结

Redis 以其微秒级的响应延迟和丰富的数据结构，在量化交易系统中扮演着实时数据中枢的关键角色。从行情缓存到信号传递，从分布式锁到会话管理，Redis 提供了构建高性能交易系统所需的核心基础设施。对于追求低延迟和高吞吐的量化交易系统来说，Redis 是不可或缺的内存数据库选择。
