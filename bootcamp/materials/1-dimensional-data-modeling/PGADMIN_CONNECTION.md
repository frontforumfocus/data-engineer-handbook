# Connecting pgAdmin to PostgreSQL

## The Problem
When connecting pgAdmin (running in Docker) to PostgreSQL (also in Docker), you cannot use `localhost` or `127.0.0.1` as the hostname because they refer to the pgAdmin container itself, not the PostgreSQL container.

## The Solution
Use the **Docker service name** as the hostname. In this setup, the PostgreSQL service is named `postgres`, so use that as the hostname.

## Step-by-Step Instructions

1. **Access pgAdmin**
   - Open your browser and go to: `http://localhost:5050`
   - Login with:
     - Email: `postgres@postgres.com` (or your `PGADMIN_EMAIL` from `.env`)
     - Password: `postgres` (or your `PGADMIN_PASSWORD` from `.env`)

2. **Add a New Server**
   - Right-click on "Servers" in the left panel
   - Select "Register" → "Server..."

3. **Configure the Connection**
   - **General Tab:**
     - Name: `PostgreSQL` (or any name you prefer)
   
   - **Connection Tab:**
     - **Host name/address:** `postgres` ⚠️ **This is the key! Use the service name, NOT localhost**
     - **Port:** `5432`
     - **Maintenance database:** `postgres` (or your `POSTGRES_DB` from `.env`)
     - **Username:** `postgres` (or your `POSTGRES_USER` from `.env`)
     - **Password:** `postgres` (or your `POSTGRES_PASSWORD` from `.env`)
     - Check "Save password" if you want pgAdmin to remember it
   
   - Click "Save"

4. **Verify Connection**
   - The server should appear in the left panel
   - You should be able to expand it and see databases, tables, etc.

## Connection Details Summary

| Setting | Value | Notes |
|---------|-------|-------|
| Host | `postgres` | **Must be the service name from docker-compose.yml** |
| Port | `5432` | Default PostgreSQL port |
| Database | `postgres` | Check your `.env` file for `POSTGRES_DB` |
| Username | `postgres` | Check your `.env` file for `POSTGRES_USER` |
| Password | `postgres` | Check your `.env` file for `POSTGRES_PASSWORD` |

## Why This Works

Docker Compose automatically creates a network for all services in the same `docker-compose.yml` file. Services can communicate with each other using their service names as hostnames. So:
- `postgres` resolves to the PostgreSQL container's IP address
- `localhost` or `127.0.0.1` would refer to the pgAdmin container itself (not PostgreSQL)

## Troubleshooting

If you still can't connect:
1. Verify both containers are running: `docker compose ps`
2. Check PostgreSQL logs: `docker compose logs postgres`
3. Check pgAdmin logs: `docker compose logs pgadmin`
4. Verify the service name matches exactly (case-sensitive): `postgres`
5. Ensure you're using the correct credentials from your `.env` file

