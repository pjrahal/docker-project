# Docker Project Documentation
## Philip Rahal
## 17 November 2024

This project was completed in Ubuntu.
# PART 1: INSTALLING DOCKER:
# Step 1: Set up Docker's apt repository
Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# Step 2: Install Docker Packages
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Step 3: Verify that the installation is successful by running the hello-world image
sudo docker run hello-world

# Step 4: Download the latest DEB Package
sudo apt-get update
sudo apt-get install /home/pjrahal/Downloads/docker-desktop-amd64.deb

# Step 5: Installing Docker Compose
sudo apt update
sudo apt install docker-compose-plugin

## Step 5b: Testing Docker Compose Install
docker compose version
Which gave the following output:
Docker Compose version v2.29.7-desktop.1

# PART 2: INSTALLING WORDPRESS:
# Step 1: Set up Project Directory
mkdir wordpress
cd wordpress

# Step 2: Create a Docker Compose File
sudo nano docker-compose.yml

## Step 2b: Populate the Docker Compose File (Replace usernames and passwords)
version: "3" 
services:
  db:
    image: mysql:latest
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: MySQLRootPassword
      MYSQL_DATABASE: MySQLDatabaseName
      MYSQL_USER: MySQLUsername
      MYSQL_PASSWORD: MySQLUserPassword

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    restart: always
    ports:
      - "80:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: MySQLUsername
      WORDPRESS_DB_PASSWORD: MySQLUserPassword
      WORDPRESS_DB_NAME: MySQLDatabaseName
    volumes:
      - "./:/var/www/html"

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    restart: always
    ports:
      - "8080:80"
    environment:
      PMA_HOST: db
      PMA_USER: MySQLUsername
      PMA_PASSWORD: MySQLUserPassword

volumes:
  mysql: {}

# Step 3: Start the Docker Container
sudo docker-compose up -d
