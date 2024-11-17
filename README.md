**Defibrillator Service**

Defibrillator is a lightweight utility designed to monitor and automatically restart Docker containers that are in an unhealthy state. This service ensures high availability by regularly checking the health status of containers and recovering them when necessary.

Why I need this?

Native docker without swarm is unable to detect if some container is broken and need to do auto recover, this is where this service come to fill the gap

---

**Features**

Automatic Recovery: Restarts Docker containers flagged as unhealthy by their health checks.
Lightweight & Secure: Operates with minimal permissions, dropping all unnecessary capabilities for enhanced security.
Configurable: Runs on a simple loop with a default interval of 10 minutes (600 seconds), which can be adjusted as needed.

---

**How It Works**

Health Check: The service periodically checks all running Docker containers for a health=unhealthy status.
Action: If any unhealthy containers are detected, they are restarted using docker restart.
Loop: This check-and-restart process repeats indefinitely to ensure continuous monitoring.

---

**How I can create checks in my container?**

Here is few examples what to add to your container in order to have proper healthcheck, edit port numbers per container/service needs

healthcheck of container using "curl"

```
healthcheck:
    test: ["CMD-SHELL", "curl --fail -sS -I http://127.0.0.1:80 || exit 1"]
    interval: 1m
    timeout: 2s
    retries: 5
    start_period: 15s
restart: unless-stopped
```

healthcheck of container using "wget"

```
healthcheck:
  test: ["CMD-SHELL", "wget --spider -q 127.0.0.1:80"]
  interval: 1m
  timeout: 2s
  retries: 5
  start_period: 15s
restart: unless-stopped
```

healthcheck of database container (mysql/mariadb example)

```
healthcheck:
  test: ["CMD-SHELL", "mysqladmin ping -h 127.0.0.1 -uroot -proot || exit 1"]
  interval: 1m
  timeout: 2s
  retries: 5
  start_period: 15s
restart: unless-stopped
```
