# Zabbix Stack (Docker Compose)

This repository provides a full **Zabbix 7.0 monitoring stack** using Docker Compose.  
It includes the following components:
- **PostgreSQL** – Database for Zabbix
- **Zabbix Server**
- **Zabbix Web Frontend** (Nginx + PHP-FPM)
- **Zabbix Agent2**

This setup is ideal for lab environments, testing, or small production deployments.

---

## Repository Structure
```
zabbix-stack/
├─ docker-compose.yml   # Docker Compose configuration
├─ .env.example         # Environment variables template
├─ data/
│  ├─ alertscripts/     # Custom alert scripts (mounted into container)
│  ├─ externalscripts/  # External scripts (mounted into container)
│  └─ postgres/         # PostgreSQL persistent data
└─ README.md
```

---

## Quick Start

### 1. Clone the repository
```bash
git clone https://github.com/nghianguyen98/zabbix-stack.git
cd zabbix-stack
```

### 2. Configure environment variables
```bash
cp .env.example .env
nano .env   # Edit passwords, hostname, timezone, etc.
```

### 3. Create required folders
```bash
mkdir -p data/postgres data/alertscripts data/externalscripts
```

### 4. Start the stack
```bash
docker compose pull
docker compose up -d
```

---

## Access the Web Interface
- URL: `http://<your-host-ip>:8080`
- Default credentials:
  - **Username:** `Admin`
  - **Password:** `zabbix`

> Make sure to change the default password after the first login.

---

## Environment Variables (`.env`)
| Variable            | Description                            | Example               |
|---------------------|----------------------------------------|-----------------------|
| `POSTGRES_USER`     | PostgreSQL username                     | `zabbix`              |
| `POSTGRES_PASSWORD` | PostgreSQL password (change this)        | `change_me_123`       |
| `POSTGRES_DB`       | Database name                           | `zabbix`              |
| `TZ`                | Timezone                                | `Asia/Ho_Chi_Minh`    |
| `ZBX_HOSTNAME`      | Hostname of the agent in Zabbix          | `zabbix-docker-host`  |

---

## Useful Commands
```bash
# Check running containers
docker compose ps

# View logs
docker compose logs -f

# Stop services (data is preserved)
docker compose down

# Upgrade images and restart
docker compose pull && docker compose up -d
```

---

## Notes
- Database data is persisted in `data/postgres`. Back it up regularly to prevent data loss.
- Custom scripts:
  - Place custom alert scripts in `data/alertscripts` (mapped to `/usr/lib/zabbix/alertscripts`).
  - Place external scripts in `data/externalscripts` (mapped to `/usr/lib/zabbix/externalscripts`).
- If exposing the web interface to the internet, secure it using a reverse proxy and TLS.
