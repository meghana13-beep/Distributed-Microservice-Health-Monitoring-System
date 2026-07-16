Distributed Microservice Health Monitoring System
A distributed systems project that monitors multiple Flask-based microservices in real time. The system collects CPU usage, memory usage, response time, and service status, then displays the health data through a browser dashboard.
The project demonstrates key distributed systems concepts such as heartbeat-based failure detection, active health checks, fault tolerance, backup monitoring, alert replication, and real-time service visualization.
Project Title: Distributed Microservice Health Monitoring System
Features
Real-time monitoring for six independent microservices.
Primary monitor that collects and stores service health metrics.
Backup monitor that receives replicated metrics and takes over when the primary monitor fails.
Heartbeat protocol for service availability tracking.
Active health checks for detecting unreachable services.
Alerts for high CPU usage, high memory usage, high response time, service failure, and recovery.
SQLite-based metric and alert storage.
Web dashboard with service cards, CPU chart, uptime chart, latency history, benchmark summary, and alert history.
System Architecture
The system is divided into four layers:
Microservice Layer
Contains six independent Flask services.
Each service exposes a /health endpoint.
Services send heartbeat data to the monitors every few seconds.

Monitoring Layer
The primary monitor runs on port 8000.
It receives heartbeats, performs active health checks, stores metrics, and generates alerts.

Fault Tolerance Layer
The backup monitor runs on port 8001.
It receives replicated metrics and alerts from the primary monitor.
If the primary monitor becomes unavailable, the backup monitor promotes itself to leader mode.

Visualization Layer
The dashboard runs on port 3000.
It reads monitor API data and displays real-time service health.

Tech Stack
Python
Flask
Flask-CORS
Requests
psutil
SQLite
HTML, CSS, JavaScript
Chart.js
Project Structure
Distributed Systems Project/
+-- dashboard/
|   +-- index.html
+-- database/
|   +-- health.db
|   +-- health_backup.db
+-- monitor/
|   +-- monitor.py
|   +-- monitor_backup.py
+-- services/
|   +-- service1.py
|   +-- service2.py
|   +-- service3.py
|   +-- service4.py
|   +-- service5.py
|   +-- service6.py
+-- alerts.log
+-- requirements.txt
+-- run_all.py
Installation
Clone or extract the project folder.

Open a terminal in the project directory.

Create and activate a virtual environment.

python -m venv .venv
On Windows:
.venv\Scripts\activate
On macOS/Linux:
source .venv/bin/activate
Install dependencies.
pip install -r requirements.txt
Running the Project
Start the complete system with:
python run_all.py
This starts:
Component	Port
Primary Monitor	8000
Backup Monitor	8001
Service 1	5001
Service 2	5002
Service 3	5003
Service 4	5004
Service 5	5005
Service 6	5006
Dashboard	3000

After startup, open the dashboard:
http://localhost:3000
To stop the system, press Ctrl+C in the terminal running run_all.py.
API Endpoints
Microservices
Each service exposes:
GET http://localhost:5001/health
GET http://localhost:5002/health
GET http://localhost:5003/health
GET http://localhost:5004/health
GET http://localhost:5005/health
GET http://localhost:5006/health
Example response:
{
  "service_id": "service1",
  "status": "UP",
  "cpu_percent": 42.5,
  "memory_percent": 50.2,
  "response_time": 0.35,
  "queue_depth": 3,
  "request_count": 12
}
Primary Monitor
GET  http://localhost:8000/health
POST http://localhost:8000/heartbeat
GET  http://localhost:8000/api/status
GET  http://localhost:8000/api/alerts
GET  http://localhost:8000/api/history
GET  http://localhost:8000/api/uptime
GET  http://localhost:8000/api/benchmarks
Backup Monitor
GET  http://localhost:8001/health
POST http://localhost:8001/heartbeat
POST http://localhost:8001/replicate
POST http://localhost:8001/replicate_alert
GET  http://localhost:8001/api/status
GET  http://localhost:8001/api/alerts
GET  http://localhost:8001/api/history
GET  http://localhost:8001/api/uptime
GET  http://localhost:8001/api/benchmarks
How Fault Tolerance Works
The primary monitor normally acts as the leader. It receives heartbeats from services, performs active health checks, writes metrics to database/health.db, and replicates metrics and alerts to the backup monitor.
The backup monitor continuously checks the primary monitor using its /health endpoint. If the primary monitor becomes unreachable, the backup monitor promotes itself to leader mode and continues monitoring services using its replicated data and own active health checks.
Alert Conditions
The system generates alerts when:
CPU usage is greater than 80%.
Memory usage is greater than 80%.
Response time is greater than 2.0 seconds.
A service reports DOWN.
A service misses expected heartbeats.
The primary monitor becomes unreachable.
A failed service or monitor recovers.
Alerts are stored in SQLite and written to alerts.log.
Testing the System
Run the project and open the dashboard. The services simulate load changes and occasional failures, so the dashboard should update automatically.
You can also test endpoints manually:
curl http://localhost:8000/api/status
curl http://localhost:8000/api/alerts
curl http://localhost:8000/api/benchmarks
To test failover, stop the primary monitor process. The backup monitor should detect that port 8000 is unavailable and switch to leader mode.
Current Limitations
The primary monitor can become a bottleneck as the number of services increases.
Continuous HTTP polling and heartbeat traffic create network overhead.
Failover may cause a short monitoring gap.
Alerting is threshold-based and does not use anomaly detection.
Recovery actions are not automated beyond monitoring and alerting.
SQLite is useful for local demos but is not ideal for large-scale distributed deployment.
Future Enhancements
Add a distributed message queue for scalable metric ingestion.
Replace simple threshold alerts with anomaly detection.
Add persistent centralized storage suitable for production workloads.
Improve failover with leader election or consensus.
Add authentication for monitor APIs.
Containerize services with Docker.
Add automated recovery actions for failed services.
