# Streaming Data Processing Pipeline

A real-time ingestion and processing pipeline for user events, orchestrated with Apache Kafka and Spark Streaming, and monitored with Grafana.

---

## 🎯 Objectives

1. Simulate event generation (logs, clicks, transactions).  
2. Ingest events into Kafka.  
3. Process data in real time with Spark Streaming (windowing, aggregations).  
4. Store results in PostgreSQL.  
5. Visualize metrics and alerts in Grafana.

---

## 🧰 Tech Stack

- **Messaging**: Apache Kafka (Zookeeper + brokers)  
- **Processing**: Apache Spark Streaming (PySpark)  
- **Language**: Python 3.8+  
- **Containers**: Docker & Docker Compose  
- **Database**: PostgreSQL  
- **Monitoring**: Grafana  
- **Logging/Metrics**: Prometheus (optional)  

---

## 📐 Architecture

\`\`\`text
[Event Generator] →
  └▶ Kafka Topic “events” →
      └▶ Spark Streaming Job →
          ├▶ PostgreSQL (agg/results)  
          └▶ Metrics → Prometheus → Grafana
\`\`\`

---

## 📂 Repository Structure

\`\`\`
streaming-pipeline/
├── docker-compose.yml        # Kafka + Zookeeper + Postgres + Grafana
├── data-generator/
│   └── producer.py           # Python script sending messages to Kafka
├── spark-job/
│   ├── streaming_app.py      # PySpark job that consumes, processes, and writes data
│   └── requirements.txt
├── grafana/
│   └── dashboards.json       # Dashboard export with latency & throughput charts
├── sql/
│   └── init.sql              # Table creation scripts for Postgres
├── README.md                 # This document
└── LICENSE
\`\`\`

---

## 🔧 Prerequisites

- Docker & Docker Compose  
- Java 8+ (for Kafka)  
- Python 3.8+ (if running local scripts without Docker)  

---

## 🚀 Installation and Startup

1. **Clone the repo**  
   \`\`\`bash
   git clone https://github.com/your-username/streaming-pipeline.git
   cd streaming-pipeline
   \`\`\`

2. **Bring up services**  
   \`\`\`bash
   docker-compose up -d
   \`\`\`  
   This starts:  
   - Zookeeper  
   - Kafka (broker)  
   - PostgreSQL (port 5432)  
   - Grafana (port 3000)

3. **Initialize the database**  
   \`\`\`bash
   docker exec -i streaming_postgres psql -U postgres < sql/init.sql
   \`\`\`

4. **Start the event generator**  
   \`\`\`bash
   cd data-generator
   pip install confluent-kafka
   python producer.py
   \`\`\`

5. **Run the Spark job**  
   \`\`\`bash
   cd spark-job
   pip install -r requirements.txt
   spark-submit --master local[2] streaming_app.py
   \`\`\`

6. **Import the Grafana dashboard**  
   - Open \`http://localhost:3000\` (admin/admin)  
   - “Import” → upload \`grafana/dashboards.json\`

---

## 📊 Monitoring

- **Throughput** of processed messages.  
- **Latency** (time between ingestion and sink write).  
- **Errors** (malformed messages).  
- Configure Grafana alerts (latency thresholds or job failures).

---

## 🤝 Contributing

1. Fork the repo and create a branch: \`feature/new-function\`  
2. Add tests or update \`README.md\`.  
3. Open a pull request for review.

---

## 📜 License

This project is released under the MIT License. See \`LICENSE\` for details.
