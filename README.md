# Redash Setup with Docker Compose

This repository provides a detailed guide and configuration for deploying Redash using Docker Compose. Follow the instructions below to set up Redash, initialize the database, and resolve common errors such as the `organization table not found` issue.

---

## Prerequisites

1. Docker installed on your system.
2. Docker Compose installed.
3. Basic familiarity with Docker commands.

---

## Files in This Repository

- `docker-compose.yml`: Defines the services required for Redash.

---

## Deployment Steps

### Step 1: Clone the Repository
```bash
git clone https://github.com/akshay080396/redash-docker-setup.git
cd redash-docker-setup
```

### Step 2: Launch the Services
Run the following command to bring up all the services:
```bash
docker-compose up -d
```
This will start the following containers:
- `redash_server` (Redash server)
- `redash_worker` (Redash background tasks)
- `redash_redis` (Redis for caching and task queue)
- `redash_postgres` (PostgreSQL database for metadata storage)

### Step 3: Resolve Common Error (`organization table not found`)
If you encounter the error `organization table not found`, follow these steps:

#### Step 3.1: Access the Redash Server Container
```bash
docker exec -it redash_server bash
```

#### Step 3.2: Initialize the Database
Run the following command inside the container to create the necessary database schema:
```bash
python manage.py database create_tables
```

#### Step 3.3: Restart the Redash Server Container
Exit the container (`exit`) and restart the server:
```bash
docker restart redash_server
```

### Step 4: Access Redash
Open your browser and navigate to:
```
http://localhost:5001
```
Use the interface to configure data sources and create visualizations.

---

## Common Issues

### Error: `organization table not found`
**Cause:** Missing database schema.
**Solution:** Follow Step 3 to resolve.

---

## Notes
- Ensure that the `REDASH_SECRET_KEY` and `REDASH_COOKIE_SECRET` values in the `docker-compose.yml` file are unique and secure.
- Default ports are used: PostgreSQL (`5432`), Redis (`6379`), and Redash (`5001`). Modify if necessary.

---

## License
This repository is licensed under the MIT License. See `LICENSE` for details.
