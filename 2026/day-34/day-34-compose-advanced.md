
Current Architecture
This project consists of three services:
1.WordPress Application
2.MySQL Database
3.Redis Cache

docker-compose.yml
services:
  my_sql:
    image: mysql
    restart: always
    ports:
      - "3306:3306"

    environment:
      MYSQL_ROOT_PASSWORD: Test@123
      MYSQL_DATABASE: Test_Db
      MYSQL_USER: Test
      MYSQL_PASSWORD: Test@123

    volumes:
      - my-db:/var/lib/mysql

    healthcheck:
      test:
        [
          "CMD",
          "mysqladmin",
          "ping",
          "-h",
          "localhost",
          "-u",
          "root",
          "-pTest@123"
        ]
      interval: 5s
      timeout: 5s
      retries: 5

    networks:
      - backend-network

  cache:
    image: redis:7-alpine
    container_name: redis_cache
    restart: always

    ports:
      - "6379:6379"

    volumes:
      - redis_data:/data

    networks:
      - backend-network

  wordpress:
    image: wordpress
    restart: always

    ports:
      - "8080:80"

    environment:
      WORDPRESS_DB_NAME: Test_Db
      WORDPRESS_DB_USER: Test
      WORDPRESS_DB_PASSWORD: Test@123
      WORDPRESS_DB_HOST: my_sql

      WORDPRESS_CONFIG_EXTRA: |
        define('WP_REDIS_HOST', 'cache');
        define('WP_REDIS_PORT', 6379);

    volumes:
      - wp_data:/var/www/html

    depends_on:
      my_sql:
        condition: service_healthy

      cache:
        condition: service_started

    networks:
      - backend-network
      - frontend-network

volumes:
  my-db:
  wp_data:
  redis_data:

networks:
  frontend-network:
    driver: bridge

  backend-network:
    driver: bridge
------------------------------------------------------------------------------------------------------------------------------------------------
## Task 1: Build Your Own App Stack
Create a docker-compose.yml for a 3-service stack:

A web app (use Python Flask, Node.js, or any language you know)
A database (Postgres or MySQL)
A cache (Redis)
Write a simple Dockerfile for the web app. The app doesn't need to be complex — even a "Hello World" that connects to the database is enough.

Current Implementation
Service	Image
Web Application	WordPress
Database	MySQL
Cache	Redis

------------------------------------------------------------------------------------------------------------------------------------------------
Task 2: depends_on & Healthchecks
Add depends_on to your compose file so the app starts after the database
Add a healthcheck on the database service
Use depends_on with condition: service_healthy so the app waits for the database to be truly ready, not just started
Test: Bring everything down and up — does the app wait for the DB?

Current Configuration
depends_on:
  my_sql:
    condition: service_healthy

  cache:
    condition: service_started
Healthcheck
healthcheck:
  test:
    ["CMD","mysqladmin","ping","-h","localhost","-u","root","-pTest@123"]
  interval: 5s
  timeout: 5s
  retries: 5

Command:
docker compose up
Output:
Container day-34-my_sql-1 Waiting

Container day-34-my_sql-1 Healthy

wordpress-1 started
Observation
WordPress waits until MySQL becomes healthy before starting.

------------------------------------------------------------------------------------------------------------------------------------------------

Task 3: Restart Policies
Add restart: always to your database service
Manually kill the database container — does it come back?
Try restart: on-failure — how is it different?
Write in your notes: When would you use each restart policy?
Test

Kill MySQL container:
docker kill day-34-my_sql-1
Verify:
docker ps
Expected:
day-34-my_sql-1 Up
Container automatically restarts.

Restart Policy Notes
restart: no
Used mainly during development.

restart: always
Used for:
Databases
Production containers

restart: unless-stopped
Used for:
Long-running services

restart: on-failure
Used for:
Applications which may crash

------------------------------------------------------------------------------------------------------------------------------------------------

Task 4 : Custom Dockerfile
Example Using build
app:
  build: ./app
Dockerfile:
FROM python:3.13-slim

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

CMD ["python","app.py"]
Rebuild:
docker compose up -d --build

------------------------------------------------------------------------------------------------------------------------------------------------
Task 5 : Named Networks and Volumes
Networks
networks:
  frontend-network:
    driver: bridge

  backend-network:
    driver: bridge
Volumes
volumes:
  my-db:
  wp_data:
  redis_data:

Persistent Data
Even after:
docker compose down
Data remains because named volumes are used.

------------------------------------------------------------------------------------------------------------------------------------------------
Task 6 : Scaling
Command:
docker compose up --scale wordpress=3
Problem
All replicas try to bind:
ports:
  - "8080:80"
Error:
Bind for 0.0.0.0:8080 failed:
port is already allocated
Reason
Only one container can use host port 8080.


