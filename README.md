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

### Step 2: Generate Secure Secrets
For security reasons, Redash requires unique secrets for REDASH_SECRET_KEY and REDASH_COOKIE_SECRET. You can generate these using Python's secrets module:
import secrets

# Generate secure random secrets for REDASH
redash_secret_key = secrets.token_urlsafe(32)  # For REDASH_SECRET_KEY
redash_cookie_secret = secrets.token_urlsafe(32)  # For REDASH_COOKIE_SECRET

# Print the generated secrets
print("REDASH_SECRET_KEY:", redash_secret_key)
print("REDASH_COOKIE_SECRET:", redash_cookie_secret)

### Step 3: Launch the Services
After generating the secrets, open the docker-compose.yml file and replace the placeholder values for REDASH_SECRET_KEY and REDASH_COOKIE_SECRET with the secrets you generated in Step 2:
services:
  redash:
    environment:
      - REDASH_SECRET_KEY: "your-generated-secret-for-redash-secret-key"
      - REDASH_COOKIE_SECRET: "your-generated-secret-for-redash-cookie-secret"

### step 4: Run the following command to bring up all the services:
```bash
docker-compose up -d
```
This will start the following containers:
- `redash_server` (Redash server)
- `redash_worker` (Redash background tasks)
- `redash_redis` (Redis for caching and task queue)
- `redash_postgres` (PostgreSQL database for metadata storage)

### Step 5: Resolve Common Error (`organization table not found`)
If you encounter the error `organization table not found`, follow these steps:

#### Step 5.1: Access the Redash Server Container
```bash
docker exec -it redash_server bash
```

#### Step 5.2: Initialize the Database
Run the following command inside the container to create the necessary database schema:
```bash
python manage.py database create_tables
```

#### Step 5.3: Restart the Redash Server Container
Exit the container (`exit`) and restart the server:
```bash
docker restart redash_server
```

### Step 6: Access Redash
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
