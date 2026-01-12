# Kafka Broker Tuning & Sizing Guide (Mastery Level)

This document covers production-grade cluster management in the Confluent ecosystem.

## 🏗️ 1. Broker Sizing (The "Magic" Formula)
When sizing a production broker for high throughput:
- **CPU**: Kafka is primarily I/O bound, but TLS and Compression (Snappy/Zstd) consume significant CPU. 
  - *Rule:* 1 CPU core per 50MB/sec of throughput.
- **RAM**: Most RAM should go to the **OS Page Cache**, not the JVM Heap.
  - *Rule:* 4-8GB for JVM Heap; 32GB+ for Page Cache to keep segments in memory.
- **I/O**: Use NVMe SSDs. RAID 10 is the standard for high-performance durability.

## ⚙️ 2. ISR (In-Sync Replicas) Tuning
To guarantee zero data loss (`acks=all`):
- `min.insync.replicas=2`: If you have 3 replicas, this ensures that at least 2 are in-sync before acknowledging a write.
- `unclean.leader.election.enable=false`: Never allow a "stale" follower to become leader. Prefer downtime over data loss.

## 🚀 3. Partition Strategy
- **Throughput**: 1 Partition can handle ~10-20MB/s.
- **Key-Hashing**: Use consistent hashing (default) to ensure all messages for the same keys go to the same partition (Ordering guarantee).
- **Skew**: Use `kafka-log-dirs` to identify disk skew and `kafka-reassign-partitions` to balance them.

## 🛠️ 4. KRaft vs ZooKeeper (The Big Shift)
- **ZooKeeper (Legacy)**: External metadata store. Bottles out at ~100k partitions.
- **KRaft (Modern)**: Metadata is managed within Kafka using the Raft consensus.
  - *Benefit:* Recovery time after failure is reduced from minutes to seconds.
  - *Scale:* Supports millions of partitions.
