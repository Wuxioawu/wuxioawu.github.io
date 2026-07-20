---
title: "如何构建扎实的计算机基础知识"
pubDate: 2026-07-20
description: 一条直接有效的路线,系统补齐软件工程师的计算机基础
draft: false
slugId: tech/260720
---

# 计算机基础学习路线(8~12 周)

> **目标**
>
> 补齐操作系统、计算机网络、数据库、分布式系统等基础知识,为 AI Infra、后端开发、系统设计打下扎实的地基。
>
> **学习方式**
>
> - 每天投入 1~2 小时
> - 以视频课程为主
> - 每学一个知识点,结合真实项目思考(如 Redis、Kafka、Kubernetes、ChatGPT)

---

## 第一阶段:操作系统(2~3 周)

### ⭐ Berkeley CS162(首推)

YouTube 搜索:

```
CS162 Operating Systems Berkeley
```

**学习内容:**

- 进程(Process)与线程(Thread)
- CPU 调度(Scheduling)
- 同步与互斥(Synchronization)
- 虚拟内存(Virtual Memory)
- 文件系统(File System)

**学完应能回答:**

- 程序是如何运行起来的?
- 为什么线程比进程更轻量?
- 上下文切换(Context Switch)为什么慢?
- 为什么需要锁?死锁是如何产生的?
- 虚拟内存是如何工作的?

### ⭐ MIT 6.S081(进阶)

YouTube 搜索:

```
MIT 6.S081 Operating System Engineering
```

**学习内容:**

- xv6 教学内核
- 内核(Kernel)与系统调用(System Call)
- 内存管理
- 调度器(Scheduler)

**适合人群:**

- 想深入了解 Linux 内核
- 想从事 AI Infra 方向
- 想做底层系统开发

---

## 第二阶段:计算机网络(2 周)

### ⭐ Stanford CS144(强烈推荐)

YouTube 搜索:

```
Stanford CS144 Computer Networking
```

**重点内容:**

- TCP 与 IP
- DNS 与路由(Routing)
- 拥塞控制(Congestion Control)
- HTTP

**学完应能回答:**

- TCP 为什么需要三次握手,而挥手是四次?
- 为什么会出现 TIME_WAIT 状态?
- HTTP/1、HTTP/2、HTTP/3 有什么区别?
- WebSocket 为什么存在?
- gRPC 为什么快?

### Georgia Tech Computer Networking(可选)

YouTube 搜索:

```
Computer Networking Georgia Tech
```

偏理论,体系更完整,可作为补充。

---

## 第三阶段:数据库(2 周)

### ⭐ CMU 15-445(必看)

YouTube 搜索:

```
CMU 15-445 Database Systems
```

**学习内容:**

- 存储引擎(Storage Engine)与缓冲池(Buffer Pool)
- B+ 树与索引
- 事务(Transaction)
- 多版本并发控制(MVCC)
- 预写日志(WAL)与故障恢复(Recovery)

**学完应能回答:**

- MySQL 为什么快?
- PostgreSQL 与 MySQL 有什么区别?
- Redis 为什么快?
- Elasticsearch 为什么不适合做事务?

---

## 第四阶段:分布式系统(2~3 周)

### ⭐ MIT 6.824(神课)

YouTube 搜索:

```
MIT 6.824 Distributed Systems
```

**重点内容:**

- MapReduce 与 GFS
- 复制(Replication)与容错(Fault Tolerance)
- Raft 与共识算法(Consensus)

> 不必做 Lab,只看 Lecture 即可。

**学完应能回答:**

- Leader 选举是如何进行的?
- 什么是一致性?如何理解 CAP 定理?
- 为什么 Raft 比 Paxos 更流行?

### CMU Distributed Systems(可选)

YouTube 搜索:

```
CMU Distributed Systems
```

更偏工业实践,可作为补充。

---

## 第五阶段:系统设计(持续学习)

### ByteByteGo

YouTube 搜索:

```
ByteByteGo
```

**涵盖内容:** Redis、Kafka、CDN、聊天系统、Uber、YouTube、短链接服务等经典设计案例。

每集约 20 分钟,适合每天看一集。

### Gaurav Sen

YouTube 搜索:

```
Gaurav Sen
```

深入讲解大型系统的设计,例如 Netflix、WhatsApp、YouTube、Uber。

---

## 第六阶段:Linux 基础

### MIT Missing Semester

YouTube 搜索:

```
MIT Missing Semester
```

**学习内容:** Linux、Shell、Git、SSH、Vim。

建议完整看完,这些都是日常开发绕不开的工具。

---

## 第七阶段:Docker 与 Kubernetes

### TechWorld with Nana

YouTube 搜索:

```
TechWorld with Nana
```

**学习内容:** Docker、Kubernetes、Helm、CI/CD。

适合云原生入门。

---

## 第八阶段:后端原理(持续学习)

### ⭐ Hussein Nasser(强烈推荐)

YouTube 搜索:

```
Hussein Nasser
```

**推荐主题:** Redis、Kafka、PostgreSQL、WebSocket、gRPC、Nginx、CDN、负载均衡、TLS、QUIC 的工作原理。

**特点:**

- 每集约 10~20 分钟
- 非常适合工作后补基础
- 能把网络、数据库、系统设计的知识串联起来

---

## 推荐学习顺序

**前 3 周:**

1. Berkeley CS162
2. Stanford CS144

**第 4~6 周:**

3. CMU 15-445
4. MIT 6.824

**长期坚持:**

- 每天看 1 集 Hussein Nasser + 1 集 ByteByteGo(或 Gaurav Sen)
- 每周看 1 个 Docker / Kubernetes 视频

---

## 推荐课程汇总

| 推荐指数 | 课程 / 频道 | 方向 |
|---------|------------|------|
| ⭐⭐⭐⭐⭐ | Berkeley CS162 | 操作系统 |
| ⭐⭐⭐⭐⭐ | Stanford CS144 | 计算机网络 |
| ⭐⭐⭐⭐⭐ | CMU 15-445 | 数据库 |
| ⭐⭐⭐⭐⭐ | MIT 6.824 | 分布式系统 |
| ⭐⭐⭐⭐⭐ | Hussein Nasser | 后端原理 |
| ⭐⭐⭐⭐☆ | ByteByteGo | 系统设计 |
| ⭐⭐⭐⭐☆ | Gaurav Sen | 系统设计 |
| ⭐⭐⭐⭐☆ | TechWorld with Nana | Docker / Kubernetes |
| ⭐⭐⭐⭐☆ | MIT Missing Semester | Linux |
| ⭐⭐⭐⭐☆ | MIT 6.S081 | 操作系统(进阶) |

---

## 学习原则:带着问题学

每学一个知识点,都尝试用它解释一个真实系统:

| 学到的知识点 | 尝试回答的问题 |
|------------|--------------|
| 上下文切换(Context Switch) | Go 的 Goroutine 为什么切换快? |
| TCP | gRPC 为什么快? |
| Page Cache | Redis 为什么不用 Page Cache? |
| Raft | Kubernetes 为什么使用 etcd? |
| MVCC | PostgreSQL 为什么能支持高并发? |

---

## 最终目标

能够从**操作系统 → 网络 → 数据库 → 分布式系统 → 系统设计**的完整视角,解释一个复杂系统(如 ChatGPT、Redis、Kafka、Kubernetes)的设计原理,而不是停留在 API 使用层面。