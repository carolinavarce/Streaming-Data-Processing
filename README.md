# Procesamiento de Datos en Streaming

Un pipeline de ingestión y procesamiento en tiempo real de eventos de usuario, orquestado con Apache Kafka y Spark Streaming, y monitorizado con Grafana.

---

## 🎯 Objetivos

1. Simular generación de eventos (logs, clicks, transacciones).  
2. Ingestar los eventos en Kafka.  
3. Procesar en tiempo real con Spark Streaming (windowing, agregaciones).  
4. Almacenar resultados en PostgreSQL.  
5. Visualizar métricas y alertas en Grafana.

---

## 🧰 Tech Stack

- **Mensajería**: Apache Kafka (Zookeeper + brokers)  
- **Procesamiento**: Apache Spark Streaming (PySpark)  
- **Lenguaje**: Python 3.8+  
- **Contenedores**: Docker & Docker Compose  
- **Base de datos**: PostgreSQL  
- **Monitorización**: Grafana  
- **Logs / métricas**: Prometheus (opcional)  

---

## 📐 Arquitectura

\`\`\`text
[Generador de eventos] →
  └▶ Kafka Topic “events” →
      └▶ Spark Streaming Job →
          ├▶ PostgreSQL (agg/results)  
          └▶ Métricas → Prometheus → Grafana
\`\`\`

---

## 📂 Estructura del repositorio

\`\`\`
streaming-pipeline/
├── docker-compose.yml        # Kafka + Zookeeper + Postgres + Grafana
├── data-generator/
│   └── producer.py           # Script Python que envía mensajes a Kafka
├── spark-job/
│   ├── streaming_app.py      # Job PySpark que consume, procesa y escribe
│   └── requirements.txt
├── grafana/
│   └── dashboards.json       # Export de dashboard con gráficas de latencia, throughput
├── sql/
│   └── init.sql              # Creación de tablas en Postgres
├── README.md                 # Este documento
└── LICENSE
\`\`\`

---

## 🔧 Prerrequisitos

- Docker & Docker Compose  
- Java 8+ (para Kafka)  
- Python 3.8+ (si ejecutas local scripts sin Docker)  

---

## 🚀 Instalación y puesta en marcha

1. **Clona el repo**  
   \`\`\`bash
   git clone https://github.com/tu-usuario/streaming-pipeline.git
   cd streaming-pipeline
   \`\`\`

2. **Levanta los servicios**  
   \`\`\`bash
   docker-compose up -d
   \`\`\`  
   Esto arranca:  
   - Zookeeper  
   - Kafka (broker)  
   - PostgreSQL (puerto 5432)  
   - Grafana (puerto 3000)

3. **Inicializa la BD**  
   \`\`\`bash
   docker exec -i streaming_postgres psql -U postgres < sql/init.sql
   \`\`\`

4. **Arranca el generador de eventos**  
   \`\`\`bash
   cd data-generator
   pip install confluent-kafka
   python producer.py
   \`\`\`

5. **Ejecuta el job de Spark**  
   \`\`\`bash
   cd spark-job
   pip install -r requirements.txt
   spark-submit --master local[2] streaming_app.py
   \`\`\`

6. **Importa el dashboard a Grafana**  
   - Entra en \`http://localhost:3000\` (admin/admin)  
   - “Import” → sube \`grafana/dashboards.json\`

---

## 📊 Monitorización

- **Throughput** de mensajes procesados.  
- **Latencia** de procesamiento (tiempo entre ingestión y guardado).  
- **Errores** (mensajes mal formados).  
- Configura alertas en Grafana (umbral de latencia o fallo de job).

---


## 📜 Licencia

Este proyecto usa la licencia MIT. Véase \`LICENSE\`.  
