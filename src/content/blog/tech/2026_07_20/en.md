---
title: "How to Build Solid CS Fundamentals"
pubDate: 2026-07-20
description: A direct, structured roadmap to master computer science fundamentals as a software engineer
draft: false
slugId: tech/260720
---

# Computer Science Fundamentals Roadmap (8–12 Weeks)

> **Goal**
>
> Fill the gaps in operating systems, computer networking, databases, and distributed systems — building a solid foundation for AI infra, backend engineering, and system design.
>
> **How to study**
>
> - 1–2 hours per day
> - Video courses first
> - For every concept you learn, connect it to a real-world system (Redis, Kafka, Kubernetes, ChatGPT, etc.)

---

## Phase 1: Operating Systems (2–3 weeks)

### ⭐ Berkeley CS162 (Top pick)

Search on YouTube:

```
CS162 Operating Systems Berkeley
```

**Topics:**

- Processes and threads
- CPU scheduling
- Synchronization
- Virtual memory
- File systems

**Questions you should be able to answer afterwards:**

- How does a program actually run?
- Why are threads lighter than processes?
- Why is a context switch expensive?
- Why do we need locks, and how do deadlocks happen?
- How does virtual memory work?

### ⭐ MIT 6.S081 (Advanced)

Search on YouTube:

```
MIT 6.S081 Operating System Engineering
```

**Topics:**

- The xv6 teaching kernel
- Kernel and system calls
- Memory management
- The scheduler

**Best for you if:**

- You want to understand the Linux kernel
- You're aiming for AI infra
- You want to work on low-level systems

---

## Phase 2: Computer Networking (2 weeks)

### ⭐ Stanford CS144 (Highly recommended)

Search on YouTube:

```
Stanford CS144 Computer Networking
```

**Key topics:**

- TCP and IP
- DNS and routing
- Congestion control
- HTTP

**Questions you should be able to answer afterwards:**

- Why does TCP need a three-way handshake but a four-way teardown?
- Why does the TIME_WAIT state exist?
- What are the differences between HTTP/1, HTTP/2, and HTTP/3?
- Why does WebSocket exist?
- Why is gRPC fast?

### Georgia Tech Computer Networking (Optional)

Search on YouTube:

```
Computer Networking Georgia Tech
```

More theoretical and systematic — a good supplement.

---

## Phase 3: Databases (2 weeks)

### ⭐ CMU 15-445 (Must watch)

Search on YouTube:

```
CMU 15-445 Database Systems
```

**Topics:**

- Storage engines and the buffer pool
- B+ trees and indexing
- Transactions
- Multi-version concurrency control (MVCC)
- Write-ahead logging (WAL) and crash recovery

**Questions you should be able to answer afterwards:**

- Why is MySQL fast?
- What's the difference between PostgreSQL and MySQL?
- Why is Redis fast?
- Why is Elasticsearch a poor fit for transactions?

---

## Phase 4: Distributed Systems (2–3 weeks)

### ⭐ MIT 6.824 (Legendary course)

Search on YouTube:

```
MIT 6.824 Distributed Systems
```

**Key topics:**

- MapReduce and GFS
- Replication and fault tolerance
- Raft and consensus

> Skip the labs — the lectures alone are enough for this roadmap.

**Questions you should be able to answer afterwards:**

- How does leader election work?
- What is consistency, and how should you think about the CAP theorem?
- Why did Raft become more popular than Paxos?

### CMU Distributed Systems (Optional)

Search on YouTube:

```
CMU Distributed Systems
```

More industry-oriented — a good supplement.

---

## Phase 5: System Design (Ongoing)

### ByteByteGo

Search on YouTube:

```
ByteByteGo
```

**Covers:** Redis, Kafka, CDNs, chat systems, Uber, YouTube, URL shorteners, and other classic design case studies.

Each episode is about 20 minutes — perfect for one a day.

### Gaurav Sen

Search on YouTube:

```
Gaurav Sen
```

Deep dives into large-scale system design: Netflix, WhatsApp, YouTube, Uber.

---

## Phase 6: Linux Basics

### MIT Missing Semester

Search on YouTube:

```
MIT Missing Semester
```

**Topics:** Linux, shell, Git, SSH, Vim.

Watch the whole series — these are tools you'll use every single day.

---

## Phase 7: Docker & Kubernetes

### TechWorld with Nana

Search on YouTube:

```
TechWorld with Nana
```

**Topics:** Docker, Kubernetes, Helm, CI/CD.

A great entry point into cloud native.

---

## Phase 8: Backend Internals (Ongoing)

### ⭐ Hussein Nasser (Highly recommended)

Search on YouTube:

```
Hussein Nasser
```

**Recommended topics:** how Redis, Kafka, PostgreSQL, WebSocket, gRPC, Nginx, CDNs, load balancers, TLS, and QUIC work under the hood.

**Why it's great:**

- Episodes run 10–20 minutes
- Ideal for filling fundamentals gaps while working full-time
- Ties networking, databases, and system design together

---

## Suggested Study Order

**Weeks 1–3:**

1. Berkeley CS162
2. Stanford CS144

**Weeks 4–6:**

3. CMU 15-445
4. MIT 6.824

**Ongoing:**

- Daily: 1 Hussein Nasser episode + 1 ByteByteGo (or Gaurav Sen) episode
- Weekly: 1 Docker / Kubernetes video

---

## Course Summary

| Rating | Course / Channel | Area |
|--------|-----------------|------|
| ⭐⭐⭐⭐⭐ | Berkeley CS162 | Operating systems |
| ⭐⭐⭐⭐⭐ | Stanford CS144 | Computer networking |
| ⭐⭐⭐⭐⭐ | CMU 15-445 | Databases |
| ⭐⭐⭐⭐⭐ | MIT 6.824 | Distributed systems |
| ⭐⭐⭐⭐⭐ | Hussein Nasser | Backend internals |
| ⭐⭐⭐⭐☆ | ByteByteGo | System design |
| ⭐⭐⭐⭐☆ | Gaurav Sen | System design |
| ⭐⭐⭐⭐☆ | TechWorld with Nana | Docker / Kubernetes |
| ⭐⭐⭐⭐☆ | MIT Missing Semester | Linux |
| ⭐⭐⭐⭐☆ | MIT 6.S081 | Operating systems (advanced) |

---

## Study Principle: Learn With Questions

For every concept you learn, try to explain a real-world system with it:

| Concept | Question to answer |
|---------|-------------------|
| Context switching | Why are Go's goroutines so cheap to switch? |
| TCP | Why is gRPC fast? |
| Page cache | Why doesn't Redis rely on the page cache? |
| Raft | Why does Kubernetes use etcd? |
| MVCC | Why can PostgreSQL handle high concurrency? |

---

## The End Goal

Be able to explain the design of a complex system (ChatGPT, Redis, Kafka, Kubernetes) from the full stack of fundamentals — **operating systems → networking → databases → distributed systems → system design** — instead of stopping at the API level.