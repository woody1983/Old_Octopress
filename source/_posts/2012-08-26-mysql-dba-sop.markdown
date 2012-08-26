---
layout: post
title: "一个MYSQL专职DBA要掌握的"
date: 2010-05-02 17:40
comments: true
categories: MySQL
---

* `DDL DML DCL` 基础中的基础 没这个 你能在数据库上面做什么？
* `Install`  重要性相当于hello world
* `DataType` 可以理解为约束
* `+-*%` 处理数据的基础工具
* `function-inside` 内置函数 枯燥 但如果告诉你这个可以减轻你工作压力的话？
* `engine` #与系统关系最紧密的内容
* `engine-Myisam` #速度快 开销小 相对发展比较完善的引擎 应用最多
* `engine-Innodb` #对数据完整性要求高的最爱
* `charset` #跨平台时会想到这个
* `Index-design` #小数据量的前提下体会不出的重要性
* `Procedure` #封装还是封装 将查询细节封装 甚至其他的动作。
* `DIY-function` #不再重复过去的劳动
* `Trigger` #虽然是智能化的体现 但用之前要想好关联的准确性。
* `Lock` # 确保数据库的一致性 维护时的神器之一
* `optimize-table` #完整范式的表不一定是最优秀的表。
* `Transaction` #Innodb中的最爱。
* `MySQL-tools` #不一定做什么事情都需要登进MYSQL里面。
* `Logs` #二进制日志是最好的备份。
* `Backup/Recover` #这个不说了
* `Privileges` # 很简单但很重要 更新过后记得flush一下
* `Safe-config` #数据的安全。
* `Replication` #应用很广的内容之一 双机热备 主从架构等等 
* `NDB-Cluster` #比replication稍微负载一些 机器多的前提下性能发挥越高。
* `Partitioning` #千万级数据的瞬间查询完成。
* `Variables` #当改跑车一样的去调教吧。
* `DataBase-design` #文字无法描述 但却最重要 设计定格后的数据库优化 提升的幅度不会超过30%，良好的设计就是最优秀的优化方案。
