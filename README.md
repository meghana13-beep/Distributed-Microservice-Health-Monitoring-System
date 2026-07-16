# 🚀 Distributed Microservice Health Monitoring System

A **Distributed Systems** project that monitors multiple Flask-based microservices in real time. The system collects CPU usage, memory usage, response time, and service availability, then visualizes the health of the entire system through an interactive web dashboard.

The project demonstrates fundamental distributed systems concepts including **heartbeat-based failure detection**, **active health checks**, **fault tolerance**, **leader failover**, **metric replication**, and **real-time monitoring**.

---

## 📌 Features

- Real-time monitoring of **6 independent microservices**
- Heartbeat-based service availability detection
- Active health checks for unreachable services
- Primary and Backup monitor architecture
- Automatic failover when the primary monitor becomes unavailable
- Replication of metrics and alerts between monitors
- SQLite-based storage for metrics and alerts
- Interactive web dashboard with live updates
- Historical performance visualization
- Alert generation and recovery tracking

---

# 🏗️ System Architecture

```
                        +----------------------+
                        |   Web Dashboard      |
                        |     Port 3000        |
                        +----------+-----------+
                                   |
                      Fetches Monitoring Data
                                   |
          +------------------------+------------------------+
          |                                                 |
+---------v---------+                         +-------------v------------+
|  Primary Monitor  | <------Replication---->|   Backup Monitor         |
|     Port 8000     |                         |      Port 8001           |
+---------+---------+                         +-------------+------------+
          |                                                 |
          | Active Health Checks                            |
          | Heartbeats                                      |
          |                                                 |
 -------------------------------------------------------------
          |         |          |         |         |         |
      +---v--+  +---v--+  +----v-+  +----v-+  +----v-+  +----v-+
      |Svc 1 |  |Svc 2 |  |Svc 3 |  |Svc 4 |  |Svc 5 |  |Svc 6 |
      |5001  |  |5002  |  |5003  |  |5004  |  |5005  |  |5006  |
      +------+  +------+  +------+  +------+  +------+  +------+
```

---

# ⚙️ Distributed Systems Concepts Demonstrated

- Heartbeat Protocol
- Failure Detection
- Active Health Checking
- Leader-Backup Architecture
- Data Replication
- Automatic Failover
- Fault Tolerance
- Health Monitoring
- Real-Time Visualization
- Distributed Alert Management

---

# 🛠️ Tech Stack

### Backend

- Python
- Flask
- Flask-CORS
- Requests
- psutil

### Database

- SQLite

### Frontend

- HTML
- CSS
- JavaScript
- Chart.js

---

# 📂 Project Structure

```
Distributed-Microservice-Health-Monitoring-System/

│
├── dashboard/
│   └── index.html
│
├── database/
│   ├── health.db
│   └── health_backup.db
│
├── monitor/
│   ├── monitor.py
│   └── monitor_backup.py
│
├── services/
│   ├── service1.py
│   ├── service2.py
│   ├── service3.py
│   ├── service4.py
│   ├── service5.py
│   └── service6.py
│
├── alerts.log
├── requirements.txt
├── run_all.py
└── README.md
```

---

# 🚀 Installation

Clone the repository

```bash
git clone https://github.com/yourusername/Distributed-Microservice-Health-Monitoring-System.git

cd Distributed-Microservice-Health-Monitoring-System
```

Create a virtual environment

### Windows

```bash
python -m venv .venv
.venv\Scripts\activate
```

### macOS/Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install dependencies

```bash
pip install -r requirements.txt
```

---

# ▶️ Running the Project

Start the complete system:

```bash
python run_all.py
```

This launches:

| Component | Port |
|-----------|------|
| Primary Monitor | 8000 |
| Backup Monitor | 8001 |
| Dashboard | 3000 |
| Service 1 | 5001 |
| Service 2 | 5002 |
| Service 3 | 5003 |
| Service 4 | 5004 |
| Service 5 | 5005 |
| Service 6 | 5006 |

Open your browser:

```
http://localhost:3000
```

Stop the project using:

```
Ctrl + C
```

---

# 📡 REST API

## Microservices

Each service exposes:

```
GET /health
```

Example:

```json
{
  "service_id": "service1",
  "status": "UP",
  "cpu_percent": 42.5,
  "memory_percent": 50.2,
  "response_time": 0.35,
  "queue_depth": 3,
  "request_count": 12
}
```

---

## Primary Monitor

| Method | Endpoint |
|---------|----------|
| GET | /health |
| POST | /heartbeat |
| GET | /api/status |
| GET | /api/history |
| GET | /api/alerts |
| GET | /api/uptime |
| GET | /api/benchmarks |

---

## Backup Monitor

| Method | Endpoint |
|---------|----------|
| GET | /health |
| POST | /heartbeat |
| POST | /replicate |
| POST | /replicate_alert |
| GET | /api/status |
| GET | /api/history |
| GET | /api/alerts |
| GET | /api/uptime |
| GET | /api/benchmarks |

---

# ❤️ How Fault Tolerance Works

1. All services periodically send heartbeat messages.
2. The Primary Monitor stores service metrics.
3. Metrics and alerts are replicated to the Backup Monitor.
4. The Backup Monitor continuously checks the Primary Monitor.
5. If the Primary Monitor becomes unavailable, the Backup Monitor automatically promotes itself to **Leader Mode**.
6. Monitoring continues without requiring manual intervention.

---

# 🚨 Alert Conditions

The system generates alerts when:

- CPU Usage > **80%**
- Memory Usage > **80%**
- Response Time > **2 seconds**
- Service reports **DOWN**
- Heartbeat timeout occurs
- Primary monitor fails
- Failed service recovers
- Primary monitor recovers

All alerts are

- Stored in SQLite
- Logged to `alerts.log`

---

# 📊 Dashboard Features

The web dashboard displays:

- 🟢 Live service status
- 📈 CPU utilization chart
- 📉 Memory utilization
- ⚡ Response time history
- ⏱️ Uptime statistics
- 📋 Benchmark summary
- 🚨 Alert history
- 🔄 Automatic refresh

---

# 🧪 Testing

### Check service status

```bash
curl http://localhost:8000/api/status
```

### Check alerts

```bash
curl http://localhost:8000/api/alerts
```

### Check benchmarks

```bash
curl http://localhost:8000/api/benchmarks
```

---

## Testing Failover

1. Start the project.
2. Stop the Primary Monitor.
3. Wait a few seconds.
4. The Backup Monitor detects the failure.
5. Backup promotes itself to **Leader Mode**.
6. Monitoring continues seamlessly.

---

# ⚠️ Current Limitations

- Primary monitor can become a bottleneck as services increase.
- Continuous heartbeat traffic introduces network overhead.
- Brief monitoring interruption during failover.
- Threshold-based alerting only.
- No automatic recovery actions.
- SQLite is suitable for demonstrations but not large-scale deployments.

---

# 🔮 Future Enhancements

- Kubernetes deployment
- Docker containerization
- Prometheus integration
- Grafana dashboards
- RabbitMQ/Kafka for asynchronous messaging
- AI/ML-based anomaly detection
- Email/SMS/Slack notifications
- Distributed consensus using Raft
- Horizontal monitor scaling
- Authentication and role-based access control
- Cloud deployment on AWS/Azure/GCP
- Time-series databases (InfluxDB/TimescaleDB)

---

# 📚 Learning Outcomes

This project demonstrates practical implementation of:

- Distributed Systems
- Fault Tolerance
- Leader Election Concepts
- Failure Detection
- Health Monitoring
- Service Replication
- REST APIs
- Flask Microservices
- Real-Time Dashboards
- Database Integration

---

# 👨‍💻 Author

**Meghana Sai**

B.Tech Computer Science Engineering  
Amrita Vishwa Vidyapeetham

---

## ⭐ If you found this project useful, consider giving it a star!
Add a distributed message queue for scalable metric ingestion.
Replace simple threshold alerts with anomaly detection.
Add persistent centralized storage suitable for production workloads.
Improve failover with leader election or consensus.
Add authentication for monitor APIs.
Containerize services with Docker.
Add automated recovery actions for failed services.
