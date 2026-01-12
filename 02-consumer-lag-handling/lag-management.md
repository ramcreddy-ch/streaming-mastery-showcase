# Consumer Lag Handling at Enterprise Scale

Consumer lag is inevitable; handling it gracefully is the mark of a senior engineer.

## 🚨 1. Monitoring Lag
Use **Burrow** or Confluent Control Center. Monitor `records-lag-max` and `records-lag-avg`.

## 🛠️ 2. Handling Strategies
1. **Parallel Consumption**: 
   - Increase partitions to match consumer instances ($Partitions >= Consumers$).
   - Use the **Confluent Parallel Consumer** library to process keys in parallel within a single partition (advanced).
2. **Batch Optimization**:
   - `fetch.min.bytes`: Set higher to increase throughput (at the cost of latency).
   - `max.poll.records`: Increase the number of records returned per poll for high-velocity streams.
3. **Backpressure**:
   - Never use "unbounded" buffers. If the DB/Sink is slow, let it propagate back to the consumer poll loop to slow down the fetch rate.

## 💊 3. The "Poison Pill" Pattern
When a record crashes the consumer (`SerializationException`):
- **Pattern**: Redirect to a **Dead Letter Queue (DLQ)**.
- **Implementation**: Do NOT block the whole partition. Commit the offset and alert the SRE team.
