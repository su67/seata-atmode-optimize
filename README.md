## seata-atmode-optimize

#### 前言

一直以来，我的学习方式局限于理论知识的学习，代码没敲几行，八股文倒是背了不少。思来想去，觉得十分不妥，深究开源项目是非常有必要的，于是有了接下来的flag。项目原型取自 [开源之夏](https://summer-ospp.ac.cn/)，技术领域是云原生、数据库、分布式系统，编程语言用到了Java、Lua。

#### 项目描述

在seata 计算与存储分离下，redis成为炙手可热的Seata事务存储模式，但再此模式下，使用的是java + jedis(pipeline, mulit)的处理，导致在server端断电，宕机等情况下，会因为多次网络io及计算下才算一次完整动作的缘故导致如全局锁被残留时，会影响AT模式下部分数据的处理，而改为lua脚本进行时可将一系列计算+存储交由redis进行，保证了tc存储数据和争抢全局锁时的原子性，保证redis事务存储模式下的高可用，一致性。

#### 技术方法

Seata 默认是 AT 模式，其中保证 AT 的最重要的是写隔离机制：一阶段本地事务提交前，需要确保拿到全局锁，拿不到全局锁则不能提交事务。所以，为保证 tc 存储数据和争抢全局锁时的原子性，必须将一系列计算+存储交由 Redis 进行，保证一个完整动作的原子性。在这一过程中需要基于 Lua 实现计数器限流，根据 rateLimiter.lua 生成 redisScript，设置好返回值并执行。

#### 时间规划

第一阶段（8月-10月）：

- 完成对于 Seata 的事务存储模式的流程的梳理
- 熟悉相关 sessionManager 的功能
- Redis 和 Lua 结合的扎实知识

第二阶段（11月-1月）：

- 完成 Seata 下所有 Redis 相关动作的 Lua 化
- 确保所有计算与存储皆在 Lua 脚本中执行
- 完成集成测试

