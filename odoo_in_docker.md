
# Odoo 16 in Docker

## Technical Odoo 16 Odoo Community

Installing and deploying Odoo 16 on Ubuntu doesn't have to be complicated. In this blog post, we will walk you through a simplified installation process using Docker, a containerization platform that ensures a hassle-free setup and enhances compatibility.

## Why Docker?

Docker provides a lightweight and isolated environment for running applications like Odoo. By using Docker, you can eliminate compatibility issues and easily manage dependencies, resulting in a smoother and more efficient deployment.

Additionally, Docker's isolation capabilities provide security and prevent interference between Odoo and other applications or services running on the same system.

Two Docker containers are required to run Odoo 16 in Docker. The containers can be created using the Official Odoo image and Postgres image from the docker hub.

Before proceeding with the clean installation of Docker, it is recommended to remove any conflicting packages:
```
sudo apt-get remove docker docker-engine docker.io docker-doc docker-compose podman-docker containerd runc  
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
```
Also, the images, containers, volumes, custom configuration files, etc. are not removed automatically. To remove these:
```
sudo rm -rf /var/lib/docker  
sudo rm -rf /var/lib/containerd
```
### Installation

Update the package index:
```
sudo apt-get update
```
Install necessary packages to enable access to the Docker repositories over HTTPS:
```
sudo apt-get install apt-transport-https ca-certificates curl gnupg software-properties-common
```
Add Docker’s official GPG key to ensure the authenticity and integrity of Docker packages:
```
sudo mkdir -p /etc/apt/keyrings  
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \  
```
```
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
Add the Docker repository to the package manager's sources list:
```
echo \  
"deb [arch="$(dpkg --print-architecture)" \  
signed-by=/etc/apt/keyrings/docker.gpg] \  
https://download.docker.com/linux/ubuntu \  
"$(. /etc/os-release && echo "$UBUNTU_CODENAME")" stable" | \  
```
```
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
The following command (for Debian-based system) ensures the source to install it is from the Docker repo instead of the default Ubuntu repo:
```
apt-cache policy docker-ce
```
Update the local package index:
```
sudo apt-get update
```
Install Docker:
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin docker-compose
```
Check if Docker is running:
```
sudo systemctl status docker
```
The following command can be used to verify that the Docker Engine installation is successful. It will create a new container from a test image (‘hello-world’). The image will be downloaded from Docker Hub for the first time. A ‘Hello from Docker!’ is returned if everything worked correctly:
```
sudo docker run hello-world
```
Install Odoo in Docker

Create a new directory in the home directory and cd into it.
```
mkdir -p ~/docker/odoo  
```
```
cd ~/docker/odoo  
```
```
mkdir config  
```
```
mkdir custom-addons
```
### Docker Compose

It is a tool that allows you to define and manage multi-container applications.

In the docker-compose configuration file (docker-compose.yml) there is an option to specify 'env_file' which contains the 'environment' variables that can be used within the container. This provides some level of security by keeping passwords out of the docker-compose.yml file, in case of using any version-control system like git.

Configuring Postgres using docker-compose

Create a ‘myenvfile.env’ file
```
nano myenvfile.env
```
Add these environment variables to it.

#postgresql env variables  
```
POSTGRES_DB=postgres #name of the PostgreSQL db that will be created or used by the PostgreSQL server.  
POSTGRES_PASSWORD=odoo16 #db_user  
POSTGRES_USER=odoo16 #db_password  
PGDATA=/var/lib/pgsql/data/pgdata #storage location of DBs and data files of PSQL  
```
# odoo env variables  
```
HOST=postgres  
USER=odoo16  
PASSWORD=odoo16
```
Create the docker-compose.yml file
```
nano docker-compose.yml
```
Add these contents to it
```
version: '3.1'  
services:  
  odoo:  
    image: odoo:16.0  
    env_file: myenvfile.env  
    depends_on:  
      - psql  
    ports:  
      - "8069:8069" #port mapping(custom-port:8069)  
    volumes:  
      - data:/var/lib/odoo  
      - ./config:/etc/odoo  
      - ./custom-addons:/mnt/extra-addons  
  psql:  
    image: postgres:13  
    env_file: myenvfile.env  
    volumes:  
      - db:/var/lib/pgsql/data/pgdata  
volumes:  
  data:  
  db:
```
Create a custom configuration file in “./config”
```
cd config  
nano odoo.conf
```
Copy these into the editor
```
[options]  
admin_passwd = strong_admin_password  
db_host = psql  
db_user = odoo16  
db_password = odoo16  
db_port = 5432  
addons_path = /mnt/extra-addons
```
To start the Odoo instance, first, ensure the current directory is where the docker-compose.yml exists and run:
```
docker-compose up
```
When this command is executed the terminal displays the logs.

To continue using the terminal after executing the command, add the ‘-d’ flag.

It is used to start the containers in a detached mode in the background, and the terminal can be used further or can be closed without stopping the containers.
```
docker-compose up -d
```
Now the Odoo instance can be accessed by visiting http://localhost:8069
