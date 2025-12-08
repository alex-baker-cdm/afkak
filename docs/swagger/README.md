# Afkak OpenAPI/Swagger Documentation

This directory contains OpenAPI 3.0 specification documentation for the Afkak Twisted Python Kafka Client library.

## Overview

While Afkak is a Python library (not a REST API), this OpenAPI specification provides standardized documentation of the library's public API, including:

- **KafkaClient**: Cluster-aware Kafka client for connecting to brokers
- **Producer**: Message producer with batching and compression support
- **Consumer**: Single-partition message consumer
- **ConsumerGroup**: Coordinated consumer group with automatic partition assignment
- **Partitioners**: RoundRobinPartitioner and HashedPartitioner
- **Data Structures**: Request/response objects and metadata types
- **Constants**: Offset constants, compression codecs, and acknowledgement levels

## Files

- `openapi.yaml` - Complete OpenAPI 3.0 specification

## Viewing the Documentation

### Using Swagger UI

You can view this documentation using Swagger UI:

1. **Online Swagger Editor**: Visit [editor.swagger.io](https://editor.swagger.io/) and paste the contents of `openapi.yaml`

2. **Local Swagger UI**: 
   ```bash
   # Using Docker
   docker run -p 8080:8080 -e SWAGGER_JSON=/docs/openapi.yaml -v $(pwd):/docs swaggerapi/swagger-ui
   ```

3. **Using Python**:
   ```bash
   pip install connexion[swagger-ui]
   # Then serve the spec with your preferred method
   ```

### Using Redoc

```bash
# Using npx
npx redoc-cli serve openapi.yaml

# Or using Docker
docker run -p 8080:80 -v $(pwd)/openapi.yaml:/usr/share/nginx/html/openapi.yaml redocly/redoc
```

## Quick Reference

### Creating a Client

```python
from afkak.client import KafkaClient

client = KafkaClient("localhost:9092")
```

### Producing Messages

```python
from afkak.producer import Producer

producer = Producer(client)
d = producer.send_messages("my-topic", msgs=[b"Hello, Kafka!"])
```

### Consuming Messages

```python
from afkak.consumer import Consumer
from afkak.common import OFFSET_EARLIEST

def processor(consumer, messages):
    for msg in messages:
        print(msg.message.value)
    consumer.commit()

consumer = Consumer(client, "my-topic", 0, processor, consumer_group="my-group")
d = consumer.start(OFFSET_EARLIEST)
```

## Related Documentation

- [Full Afkak Documentation](https://afkak.readthedocs.io/en/latest/)
- [API Reference](../api.rst)
- [GitHub Repository](https://github.com/ciena/afkak)
