# Fineract Deployment with Docker Compose

This repository contains a `docker-compose.yml` configuration to deploy a complete **Apache Fineract** system, including its dependencies (PostgreSQL database, ActiveMQ broker, and web application). The setup allows quick and straightforward deployment for development, testing, or evaluation purposes.

---

## Prerequisites

- Docker
- Docker Compose

---

## Services

### 1. **PostgreSQL**
- **Image**: `postgres`
- **Ports**: `5432` (default PostgreSQL port exposed)
- **Volumes**:
  - `postgres_data`: Persistent storage for PostgreSQL data.
  - `init.sql`: Initializes required databases (`fineract_default`, `fineract_tenants`).
- **Environment Variables**:
  - `POSTGRES_PASSWORD`: Password for the PostgreSQL `postgres` user.

### 2. **ActiveMQ**
- **Image**: `apache/activemq-artemis:latest-alpine`
- **Ports**:
  - `61616`: For JMS messaging.
  - `8161`: Web console for monitoring ActiveMQ.
- **Network**: Connected to `fineract_network`.

### 3. **Apache Fineract**
- **Image**: `apache/fineract:latest`
- **Ports**: `8443` (default API endpoint).
- **Environment Variables**:
  - Configuration for connecting to PostgreSQL (e.g., hostname, port, credentials).
  - Disables SSL for local development (`FINERACT_SERVER_SSL_ENABLED: false`).
- **Network**: Connected to `fineract_network`.

### 4. **Web Application**
- **Image**: `openmf/web-app:master`
- **Ports**: `80` (web app UI).
- **Environment Variables**:
  - `FINERACT_API_URL`: URL of the Fineract API.
  - `FINERACT_PLATFORM_TENANT_IDENTIFIER`: Default tenant for the platform.
- **Dependency**: Starts after the `fineract` service.

---

## Network

- **fineract_network**: A dedicated bridge network for all services to enable seamless communication.

---

## Volumes

- **postgres_data**: Stores PostgreSQL data persistently.

---

## Setup

```bash
git https://github.com/minthittun/fineract-docker-setup.git
cd fineract-docker-setup
docker-compose up -d
```

## Forget the Fancy Stuff

After reading this README, you're probably thinking, "Whoa, that's a lot of fancy tech talk!" Don't worry—forget all about it. Just follow these simple steps:

- Clone the repository.
- Enter the folder.
- Run the command `docker-compose up -d`.

That's it! Sit back, grab some coffee, and let Docker do all the work. 🎉

## Browse the Web App
Once the services are up and running, you can access the web application by opening your browser and navigating to `http://localhost:80`.

Username: `mifos`
Password: `password`