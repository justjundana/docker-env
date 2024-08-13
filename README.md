# Docker Compose Project

This project utilizes Docker Compose to set up and manage multiple services including databases, monitoring tools, and an SFTP server. Below is an overview of the services included in this Docker Compose configuration.

## Services

### MySQL
- **Image**: `mysql:8.0.37`
- **Ports**: `3306:3306`
- **Volumes**:
  - `db_mysql:/var/lib/mysql`
  - `./db_bak:/db_bak`
- **Environment Variables**:
  - `MYSQL_ROOT_PASSWORD=root`
- **Command**:
  - `--log_bin_trust_function_creators=1`
  - `--max_allowed_packet=1000000000`

### MongoDB
- **Image**: `mongo:latest`
- **Ports**: `27017:27017`
- **Volumes**: `db_mongo:/data/db`
- **Environment Variables**:
  - `MONGO_INITDB_ROOT_USERNAME=root`
  - `MONGO_INITDB_ROOT_PASSWORD=root`

### Redis
- **Image**: `redis`
- **Ports**: `6379:6379`
- **Volumes**: `db_redis:/data`

### PostgreSQL
- **Image**: `postgres:latest`
- **Ports**: `5432:5432`
- **Volumes**: `db_postgres:/var/lib/postgresql/data`
- **Environment Variables**:
  - `POSTGRES_USER=root`
  - `POSTGRES_PASSWORD=root`

### Adminer
- **Image**: `adminer:latest`
- **Ports**: `8080:8080`
- **Environment Variables**:
  - `ADMINER_DEFAULT_SERVER=mysql`
- **Extra Hosts**:
  - `postgres:postgres`

### Prometheus
- **Image**: `prom/prometheus:v2.32.1`
- **Ports**: `9090:9090`
- **Volumes**:
  - `./etc/prometheus/:/etc/prometheus/`
  - `./var/prometheus/prometheus_data:/prometheus`
  - `./prometheus-custom.yml:/etc/prometheus/prometheus.yml:ro`
- **Command**:
  - `--config.file=/etc/prometheus/prometheus.yml`
  - `--storage.tsdb.path=/prometheus`
  - `--web.console.libraries=/usr/share/prometheus/console_libraries`
  - `--web.console.templates=/usr/share/prometheus/consoles`
  - `--web.enable-lifecycle`
- **Profiles**: `prometheus`

### Grafana
- **Image**: `grafana/grafana`
- **Ports**: `3000:3000`
- **Volumes**:
  - `./var/grafana/grafana_data:/var/lib/grafana`
  - `./etc/grafana/provisioning/:/etc/grafana/provisioning/`
- **Environment Variables**:
  - `GF_SECURITY_ADMIN_PASSWORD=admin`
  - `GF_USERS_ALLOW_SIGN_UP=false`
- **Profiles**: `grafana`

### Jaeger
- **Image**: `jaegertracing/all-in-one:1.39`
- **Ports**:
  - `16686:16686`
  - `4317:4317`
  - `4318:4318`
- **Environment Variables**:
  - `COLLECTOR_OTLP_ENABLED=true`

### SFTP
- **Image**: `emberstack/sftp`
- **Ports**: `2222:22`
- **Volumes**: `./config/docker/sftp.json:/app/config/sftp.json:ro`

## Volumes

- `db_mongo`
- `db_mysql`
- `db_redis`
- `db_postgres`

## Getting Started

1. **Clone the repository**:
   ```bash
   git clone https://github.com/justjundana/docker-env.git

2. **Navigate to the project directory:**:
   ```bash
   cd docker-env

3. **Start the services**:
   ```bash
   docker-compose up

4. **Access the services**:
- Adminer: http://localhost:8080
- Prometheus: http://localhost:9090
- Grafana: http://localhost:3000
- Jaeger: http://localhost:16686
- SFTP: sftp://localhost:2222

