# 🧠 Senior Streaming Engineer Interview Guide
> Deep-Dive Mechanics for 5+ YOE Professionals.

---

## 🏗️ 1. Internal Mechanics (The "Expert" Section)

### Q: "Explain the Zero-Copy principle in Kafka."
- **Answer**: Kafka uses the `sendfile()` system call. Instead of copying data from Disk -> Kernel -> User Space -> Kernel -> Network Card, it moves it directly from **Disk Buffer Cache to the Network Card**. This is why Kafka can saturate 10Gbps links with minimal CPU.

### Q: "What are Segment Files and Indexes?"
- **Answer**: Kafka logs are split into Segments (default 1GB). Every segment has a `.index` file (offsets to physical positions) and a `.timeindex` file. This allows $O(1)$ lookups when searching for an offset.

### Q: "How does KRaft handle consensus?"
- **Answer**: It uses the Raft protocol where one broker is the Metadata Leader. All metadata changes are recorded in an internal `__cluster_metadata` topic.

---

## 🌐 2. Scaling & Production Scenarios

### Scenario: "You see extreme data skew on one partition. How do you fix it?"
- **Answer**: Skew is usually caused by a poor **Message Key**. Check the cardinality of your keys. If it's too low (e.g., Status codes), add a "Salt" (random noise) or switch to a high-cardinality key like `UUID`.

### Scenario: "Explain Consumer Rebalancing and 'Sticky Assignor'."
- **Answer**: When a consumer joins/leaves, Kafka re-assigns partitions. A "Sticky Assignor" minimizes movement, ensuring a consumer keeps as many of its previously assigned partitions as possible, reducing downtime.

---

## 🔄 3. Flink & Real-time Questions

### Q: "What is the difference between Checkpoints and Savepoints?"
- **Checkpoints**: Incremental snapshots of state, automatically taken to recover from worker failures.
- **Savepoints**: Heavyweight, manual snapshots used for application upgrades or changing the Flink job's parallelism.

### Q: "How do Watermarks handle late-arriving data?"
- **Answer**: A Watermark is a heuristic that tells Flink "no data older than timestamp $X$ will arrive". If data arrives *after* the watermark has passed its window, it is dropped or handled via "Side Outputs".

---

## 📉 4. Disaster Recovery
### Q: "How do you achieve Exactly-Once Semantics (EOS)?"
- **Answer**: Use `processing.guarantee=exactly_once_v2` in Kafka Streams or the Flink Kafka Producer. It uses an internal **Transaction ID** to coordinate between the Producer and the Consumer offsets.

---
> "Interviewers don't want to know if you can write code; they want to know if you can be trusted with a 10PB cluster at 3 AM."
