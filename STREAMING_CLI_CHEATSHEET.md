# 🎯 The "Never Sleep" Streaming CLI Cheatsheet
> Kafka, Confluent, and Flink Mastery.

---

## 📦 1. Kafka Core (Basics)
| Task | Command |
| :--- | :--- |
| **List Topics** | `kafka-topics --list --bootstrap-server localhost:9092` |
| **Create Topic** | `kafka-topics --create --topic app-events --partitions 6 --replication-factor 3` |
| **Inspect Consumer Group** | `kafka-consumer-groups --describe --group my-app` |
| **Reset Offsets** | `kafka-consumer-groups --reset-offsets --to-earliest --execute --topic app-events` |

---

## 🏛️ 2. Confluent CLI (Enterprise)
| Task | Command |
| :--- | :--- |
| **Login** | `confluent login` |
| **List Clusters** | `confluent kafka cluster list` |
| **Produce (Avro)** | `confluent kafka topic produce my-topic --value-format avro --schema-id 123` |
| **View SchemaRegistry** | `confluent schema-registry schema list` |

---

## ⚡ 3. Apache Flink
| Task | Command |
| :--- | :--- |
| **List Jobs** | `flink list` |
| **Submit Job** | `flink run -d -c com.example.MyStreamingJob my-job-jar.jar` |
| **Create Savepoint** | `flink savepoint <jobID> /tmp/savepoints/` |
| **Stop with Savepoint** | `flink stop --savepointPath /tmp/savepoints/ <jobID>` |

---

## 🛠️ 4. Advanced Debugging Hacks
### Check ISR Status (Find lagging brokers)
```bash
kafka-topics --describe --under-replicated-partitions --bootstrap-server broker:9092
```

### Dump Log Segments (See raw data on disk)
```bash
kafka-run-class kafka.tools.DumpLogSegments --files /var/lib/kafka/data/topic-0/0000.log --deep-iteration
```

### Mocking CDC Events
Use `kafkacat` or `kcat` to inject raw JSON with headers to simulate complex events.
