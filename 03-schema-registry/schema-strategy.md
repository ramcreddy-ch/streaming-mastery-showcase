# Schema Governance & Evolution

Using the Confluent Schema Registry (Avro / Protobuf).

## 🧬 1. Evolution Modes
| Mode | Description | Compatibility Check |
| :--- | :--- | :--- |
| **BACKWARD** | Consumer (New) reads Producer (Old) | Default. Safest for rolling upgrades. |
| **FORWARD** | Consumer (Old) reads Producer (New) | Use when adding fields that old consumers can ignore. |
| **FULL** | Both ways | Strict. Required for high-reliability ecosystems. |

## 🛠️ 2. Best Practices
- **Never Change Types**: Changing a `long` to a `string` will break everything.
- **Default Values**: Always provide defaults for new fields to ensure backward compatibility.
- **Schema ID**: Kafka messages are prefixed with a 5-byte Magic Byte + Schema ID. This is how the Registry knows which version to fetch.
