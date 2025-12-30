# Amazon Logistics Network Real-Time Intelligent Dispatch System

A real-time intelligent dispatch system based on Confluent data streaming and Google Cloud AI, enabling dynamic optimization and matching of warehouses, vehicles, and packages.

## Structure

```
amazon/
├── data-sources/          # Data Source Layer
│   ├── kafka_topics.json  # Kafka Topics Configuration
│   ├── schemas/          # Avro Data Schemas
│   └── simulators/       # Data Simulators
├── confluent/            # Confluent Processing Layer
│   ├── ksqldb/          # ksqlDB Queries
│   └── stream_processors/ # Stream Processors
├── ai-inference/         # AI Inference Layer
│   ├── vertex_ai_service.py    # Vertex AI Service
│   ├── kafka_ai_processor.py   # Kafka AI Processor
│   └── bigquery_ml_queries.sql # BigQuery ML Queries
├── applications/         # Application Layer
│   ├── scheduler/        # Dispatch Center
│   ├── driver_app/      # Driver App API
│   ├── warehouse/       # Warehouse Alert System
│   ├── customer/        # Customer ETA Service
│   └── dashboard/       # Frontend Dashboard
├── deployment/          # Deployment Scripts
│   ├── docker-compose.yml
│   ├── deploy.sh
│   └── stop.sh
└── scripts/             # Utility Scripts
```

## Quick Start

### Prerequisites

- Docker & Docker Compose
- Python 3.9+
- Node.js 16+
- Google Cloud Platform account (optional, for AI features)
- Confluent Cloud account or local Kafka cluster

## Tech Stack

### Data Streaming

- **Kafka**: Message queue and event streaming
- **ksqlDB**: Stream SQL queries
- **Schema Registry**: Data schema management

### AI/ML

- **Vertex AI**: Model training and deployment
- **BigQuery ML**: Real-time ML queries
- **TensorFlow**: Deep learning models

### Backend Services

- **FastAPI**: Python web framework
- **WebSocket**: Real-time communication
- **PostgreSQL**: Relational database
- **Redis**: Cache and state storage

### Frontend

- **React**: UI framework
- **Material-UI**: Component library
- **Leaflet**: Map component
- **Recharts**: Chart library

## License

MIT License
