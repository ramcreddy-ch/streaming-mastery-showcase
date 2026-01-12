# Real-time Processing with Apache Flink

Flink is the gold standard for "Stateful" stream processing.

## 📊 1. Flink SQL: Windowed Aggregation
Example: Counting orders per minute in a 5-minute sliding window.

```sql
SELECT 
    window_start, 
    window_end, 
    COUNT(order_id) as total_orders
FROM TABLE(
    TUMBLE(TABLE orders, DESCRIPTOR(event_time), INTERVAL '5' MINUTES))
GROUP BY window_start, window_end;
```

## 🧠 2. Concepts You Must Know
- **Event Time vs. Processing Time**: AI models usually depend on *Event Time* (when the user clicked) to avoid data drift.
- **Watermarks**: How Flink handles "late" data arriving after the window closed.
- **Checkpoints vs. Savepoints**: 
  - *Checkpoints:* Internal self-healing.
  - *Savepoints:* Manual snapshots for application upgrades.

## 🤖 3. Flink in the AI Era
- **Feature Engineering**: Calculating real-time user features (e.g., "avg spend in last 24h") and sinking to a Feature Store.
- **Real-time Inference**: Flink can call ML models (via HTTP or gRPC) for fraud detection on every single event.
