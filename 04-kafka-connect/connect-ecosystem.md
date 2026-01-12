# Kafka Connect at Enterprise Scale

Kafka Connect is the "glue" for real-time pipelines.

## 🔌 1. Source Connectors (Getting Data In)
- **Debezium (CDC)**: Capture every `UPDATE/INSERT` from Postgres/MySQL as a streaming event.
- **S3 Source**: Ingest raw historical data back into Kafka for re-processing.

## 📥 2. Sink Connectors (Getting Data Out)
- **Snowflake/Elasticsearch**: Real-time indexing for analytics.
- **HTTP Sink**: Triggering webhooks/microservices based on Kafka events.

## 🛠️ 3. Advanced Configuration
- **SMTs (Single Message Transforms)**: Mask PII or flatten nested JSON *on-the-fly* before the record leaves the connector.
- **Dead Letter Queues (DLQ)**:
  ```json
  "errors.tolerance": "all",
  "errors.deadletterqueue.topic.name": "task-api-dlq",
  "errors.deadletterqueue.context.headers.enable": true
  ```
