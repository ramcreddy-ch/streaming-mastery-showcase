# Disaster Recovery & Replay Strategies

How to handle a Region failure in Confluent Cloud/Self-hosted.

## 🌍 1. Multi-Region Resilience
- **MirrorMaker 2 (MM2)**: The industry standard for cross-cluster replication.
- **Cluster Linking (Confluent Exclusive)**: Replicates entire topics (including offsets) between clusters with zero-latency. More powerful than MM2 for hybrid/multi-cloud.

## ⏪ 2. Replay Strategies
When a bug is found in production code, you need to "time travel":
1. **Reset Offsets**: Use `kafka-consumer-groups --reset-offsets --to-datetime ...`.
2. **Intermediate Topics**: Write the "fixed" data to a new topic to avoid side-effects on downstream consumers.
3. **Compacted Topics**: Explain how `cleanup.policy=compact` helps in state recovery by only keeping the latest key.

## ⚠️ 3. Producer Resilience
- **Idempotent Producers**: `enable.idempotence=true` prevents duplicate messages during network retries.
- **Transaction Support**: Atomic writes across multiple topics.
