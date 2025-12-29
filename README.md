# 🐘 PostgreSQL Observability Stack

This project provides a complete Dockerized environment for PostgreSQL monitoring and management using **pgAdmin 4** (Administration) and **PgHero** (Performance & Observability).

## 🚀 Quick Start

### 1. Start the Stack
Ensure you have Docker and Docker Compose installed, then run:

```bash
docker-compose up -d
```

### 2. Services Overview
Once the containers are running, you can access the tools at:

- pgAdmin: http://localhost:5050
    - Login: `admin@admin.com`
    - Password: `root`
- PgHero: http://localhost:8080

---

### 🛠️ Step 1: Connect pgAdmin to the Database
1. Open pgAdmin at http://localhost:5050.
2. Right-click Servers > Register > Server...
3. In the General tab, Name: `Test DB`.
4. In the Connection tab, enter:
    - Host name/address: `db` (This is the Docker service name)
    - Port: `5432`
    - Maintenance database: `test_db`
    - Username: `root`
    - Password: `root`
5. Click Save.

---

### 📊 Step 2: Enable Query Tracking (Required for PgHero)
PgHero requires the pg_stat_statements extension to track query history. Although it is loaded in the docker-compose config, you must enable it in the database once.

1. In pgAdmin, right-click your `test_db` and select `Query Tool`.
2. Run the following command:

```sql
CREATE EXTENSION pg_stat_statements;
```

3. Refresh PgHero (http://localhost:8080). You should see the "Query Stats" checkmark turn green.

---

### 🧪 Step 3: Simulate Slow & Heavy Queries
To see data appear in PgHero, you need to run some "expensive" queries. Execute these in the pgAdmin Query Tool:

A. Simulating a "Slow" Query (High Latency)
This query forces the database to wait for 10 seconds. It will show up in PgHero under high Average Time.

```sql
SELECT pg_sleep(10), 'Testing Latency' AS label;
```

B. Simulating a "Heavy" Query (High Load)
This generates 5 million random numbers and sums them up, putting a load on the CPU.

```sql
SELECT sum(random_num) 
FROM (SELECT random() as random_num FROM generate_series(1, 5000000)) AS data;
```

---

### 🔍 Step 4: View Results in PgHero
1. Go to the PgHero Dashboard (http://localhost:8080).
2. Click on the `Queries` tab in the top navigation.
3. What to look for:
    - `Total Time`: See which queries are eating the most CPU over time.
    - `Average Time`: See which queries make users wait the longest (the pg_sleep query will be here).
    - `Calls`: See which queries are running too frequently.

#### Pro Tip: If your queries aren't showing up immediately, click the `Reset Stats` button in the top right of PgHero to clear old noise and show only your new test queries.



# PortgresSQL & pgAdmin4 - Docker-compose

## 🔗 Reference Links

https://www.youtube.com/watch?v=qECVC6t_2mU

# Start the docker-compose
```
 docker-compose up
 ```
 
