## Task 1: Install & Verify
# Check if Docker Compose is available on your machine
# Verify the version


ubuntu@ip-172-31-14-118:~/Project/Demo$ # 1. Create the directory for CLI plugins
mkdir -p ~/.docker/cli-plugins/

# 2. Download the binary (this pulls the latest architecture-matching version)
curl -SL https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose

# 3. Give execution permissions to the downloaded file
chmod +x ~/.docker/cli-plugins/docker-compose


ubuntu@ip-172-31-14-118:~/Project/Demo$ docker compose version
Docker Compose version v5.1.4

--------------------------------------------------------------------------------------------------------------------------------------------------

## Task 2: Your First Compose File
# Create a folder compose-basics
# Write a docker-compose.yml that runs a single Nginx container with port mapping

ubuntu@ip-172-31-14-118:~/Day-33$ cat docker-compose.yml 
services :
  web :
   image : nginx:alpine
   ports :
     - "82:80"

# Start it with docker compose up

ubuntu@ip-172-31-14-118:~/Day-33$ docker compose up
[+] up 2/2
 ✔ Network day-33_default Created                                                                                                                                           0.1s
 ✔ Container day-33-web-1 Created 

# Access it in your browser

# Stop it with docker compose down

ubuntu@ip-172-31-14-118:~/Day-33$ docker compose down
[+] down 2/2
 ✔ Container day-33-web-1 Removed                                                                                                                                           0.2s
 ✔ Network day-33_default Removed 

---------------------------------------------------------------------------------------------------------------------------

## Task 3: Two-Container Setup
# Write a docker-compose.yml that runs:

# A WordPress container
# A MySQL container
# They should:

# Be on the same network (Compose does this automatically)
# MySQL should have a named volume for data persistence
# WordPress should connect to MySQL using the service name
# Start it, access WordPress in your browser, and set it up.

# Verify: Stop and restart with docker compose down and docker compose up — is your WordPress data still there? Yes

ubuntu@ip-172-31-14-118:~/Day-33/Wordpress_website$ cat docker-compose.yml 
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
      -  my-db:/var/lib/mysql
    healthcheck :
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost", "-u", "root", "-pTest@123"]
      interval: 5s
      timeout: 5s
      retries: 5

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
    volumes:
      - wp_data:/var/www/html
    depends_on:
     my_sql:
       condition: service_healthy

volumes:
  my-db:
  wp_data:
  ------------------------------------------------------------------------------------------------------------------------

## Task 4: Compose Commands
# Practice and document these:

# Start services in detached mode

docker compose up -d

# View running services

docker ps 

# View logs of all services

docker logs

# View logs of a specific service

docker logs <Container_Name>

# Stop services without removing

docker stop <Contanier_name>

# Remove everything (containers, networks)

docker compose down -v 

# Rebuild images if you make a change

docker compose up -d --build
--------------------------------------------------------------------------------------------------------------------------

## Task 5: Environment Variables
# Verify the variables are being picked up

ubuntu@ip-172-31-14-118:~/Day-33/Wordpress_website$ cat docker-compose.yml 
services:
  my_sql:
    image: mysql
    restart: always
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD} 
    volumes:
      -  my-db:/var/lib/mysql
    healthcheck :
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost", "-u", "root", "-pTest@123"]
      interval: 5s
      timeout: 5s
      retries: 5

  wordpress:
    image: wordpress
    restart: always
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_NAME: ${MYSQL_DATABASE}
      WORDPRESS_DB_USER: ${MYSQL_USER}
      WORDPRESS_DB_PASSWORD: ${MYSQL_PASSWORD}
      WORDPRESS_DB_HOST: my_sql   
    volumes:
      - wp_data:/var/www/html
    depends_on:
     my_sql:
       condition: service_healthy

volumes:
  my-db:
  wp_data:

# Create a .env file and reference variables from it in your compose file
ubuntu@ip-172-31-14-118:~/Day-33/Wordpress_website$ cat .env
MYSQL_ROOT_PASSWORD=Test@123
MYSQL_DATABASE=Test_Db
MYSQL_USER=Test
MYSQL_PASSWORD=Test@123

# Add environment variables directly in your docker-compose.yml
ubuntu@ip-172-31-14-118:~/Day-33/Wordpress_website$ docker compose config
name: wordpress_website
services:
  my_sql:
    environment:
      MYSQL_DATABASE: Test_Db
      MYSQL_PASSWORD: Test@123
      MYSQL_ROOT_PASSWORD: Test@123
      MYSQL_USER: Test