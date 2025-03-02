# 🐳 Containers with Docker

## Description
This demo project is part of **Module 7: Containers with Docker** from the **Nana DevOps Bootcamp Exercise Guide**. It focuses on containerizing a **Java application with MySQL** and deploying it using **Docker Compose** on a remote server, using DigitalOcean Droplet.
<br />

## 🚀 Technologies Used

- <b>Docker and Docker Compose: Docker for containerization.</b>
- <b>Linux: Ubuntu for Server configuration and management.</b>
- <b>Java application: Java Application developed by Nana.</b>
- <b>Gradle: Build tool for Java Application.</b>
- <b>IntelliJ: IDE for editing Java code and creating docker files.</b>
- <b>MySQL: Database. </b>
- <b>phpMyAdmin: WebUI access for MySQL database. </b>
- <b>Nexus Repository: Private Docker registry for App. </b>
- <b>Digital Ocean: Cloud provider for hosting the application.</b>
  

## 🎯 Features

- <b>Run MySQL and phpMyAdmin in local containers</b>
- <b>Use environment variables for secure database configuration.</b>
- <b>Dockerize the Java Application.</b>
- <b>Push the image to Docker repository</b>
- <b>Deploy application using docker compose on Digital Ocean</b>


## 🏗 Project Architecture
<img src="" width=800 />

## 📝 Prerequisites
- <b>Ensure that Nexus Repository from module 6 is up and running.</b>

## ⚙️ Project Configuration
### Clone the Git Repository and Create a New One
1. Clone Nana's repository.
   ```bash
   git clone <repository_link>
   ```
2. Remove existing git history: open a terminal, navigate to the cloned repository, and delete the .git folder to remove the existing version history.
3. Create your Git repository.
4. Initialize a new git repository.
   ```bash
   git init
   git add .
   git commit -m "First commit"
   git branch -m main
   git remote add origin <remote repository link>
   git push
   ```

## Start MySQL docker container
1. Open a browser and go to Docker Hub to find the official MySQL image.
   
   [DockerHub](https://hub.docker.com/_/mysql)
   
   <img src="" width=800 />
   
3. Pull the MySql image.
   
   ```bash
   docker pull mysql
   ```
   <img src="" width=800 />
   
5. Verify the downloaded image.
   
   ```bash
   docker images
   ```
   <img src="" width=800 />
   
7. Run MySQL container.
   
   ```bash
     docker run \
    --name mysql-db \
    -e MYSQL_ROOT_PASSWORD=password \
    -e MYSQL_DATABASE=mysql-db \
    -e MYSQL_USER=unicorn \
    -e MYSQL_PASSWORD=unicornpassword \
    -p 3306:3306 \
    -d mysql:latest
   ```
   <img src="" width=800 />
   
9. Verify that the container is running.

   ```bash
   docker ps
   ```
   <img src="" width=800 />

## Start phpmyAdmin docker container
1. Open a browser and go to Docker Hub to find the official phpMyAdmin image.
   
   [DockerHub](https://hub.docker.com/_/phpmyadmin)
   
   <img src="" width=800 />
   
3. Pull phpMyAdmin official image.

   ```bash
   docker pull phpmyadmin
   ```
   <img src="" width=800 />
   
5. Verify the downloaded image.

   ```bash
   docker images
   ```
   <img src="" width=800 />
   
7. Run phpMyAdmin container.

   ```bash
   docker run \
    --name phpmyadmin \
    -d \
    --link mysql-db:db \
    -p 8085:80 \
   phpmyadmin
   ```
   <img src="" width=800 />
   
9. Verify that the container is running.

   ```bash
    docker ps
    ```
   <img src="" width=800 />
   
11. Open a browser and go to phpMyAdmin.
    [phpMyAdmin](http://157.230.0.133:8085/)
    <img src="" width=800 />

## Configure Docker Compose to start Mysql & Phpmydmin together.
1. Create a YAML docker compose file in the project directory. Navigate to the root folder of the application, click on New, and file.

   <img src="" width=800 />
   
3. Define services and add a Volume to persist data from Mysql.

   ```bash
     version: '3'
    services:
      mysql-db:
        image: mysql
        ports:
          - 3306:3306
        environment:
          - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
          - MYSQL_DATABASE=mysql-db
          - MYSQL_USER=${DB_USER}
          - MYSQL_PASSWORD=${DB_PWD}
    
        volumes:
          - mysql-db-data:/var/lib/mysql
          #- ./mysql-init-scripts:/docker-entrypoint-initdb.d
    
      phpmyadmin:
        image: phpmyadmin
        ports:
          - 8085:80
        restart: always
        environment:
          - PMA_HOST=${PMA_HOST}
          - PMA_PORT=${PMA_PORT}
          - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
    
        depends_on:
          - "mysql-db"
    
    volumes:
      mysql-db-data:
        driver: local
   ```
   <img src="" width=800 />
   
5. Navigate to the root folder of the application and run Docker compose.

   ```bash
   docker compose -f docker-compose.yaml up
   ```
   <img src="" width=800 />
   
7. Verify that both containers are running.

   ```bash
   docker ps
   ```
   
   <img src="" width=800 />

## Dockerize Java Application
1. Create a Dockerfile in the root directory of the Java application.

   <img src="" width=800 />
      
3. Navigate to the project directory and build the .jar file for your application with gradle.

   ```bash
   gradle build
   ```
   <img src="" width=800 />
   
5. Build the docker image of the application.

   ```bash
   docker build -t docker-exercises:1.0 .
   ```
   <img src="" width=800 />

## Push the Docker Image to the Nexus repository
1. Open your browser and navigate to your Nexus Repository.
3. Log in to Nexus as an Admin user.
4. In the Nexus interface, go to settings, click on repositories, and create a Docker-hosted repository.
   <img src="" width=800 />
6. Navigate to security, click on role, and create a role.
7. Add the privileges to the role to access the docker hosted respository.
   <img src="" width=800 />
9. Assign the role to the user.
   <img src="" width=800 />
11. Navigate to the docker hosted repository and add the Nexus Docker adapter to allow docker clients.
    <img src="" width=800 />
13. Log in to Nexus from the command line.
    <img src="" width=800 />
    ```bash
    docker login http://157.230.56.153:8084
    ```
15. Tag the docker image with the appropriate respository name and version tag.
    ```bash
    docker tag docker-exercises:1.0 157.230.56.153:8084/docker-exercises:1.0
    ```
17. Push the image to the Nexus Docker registry.
    ```bash
    docker push docker-exercises:1.0 157.230.56.153:8084/docker-exercises:1.0
    ```
    <img src="" width=800 />
    
19. Verify that the image is available in the Docker repository on Nexus.
    <img src="" width=800 />

## Add Java application to the Docker Compose file
1. Open the existing YAML docker compose file.
2. Edit the docker compose file, and under the services section, add a new service for your java application.
   ```bash
   docker-exercises:
      image: 157.230.56.153:8084/docker-exercises:1.0
      ports:
        - 8080:8080
      environment:
        - DB_NAME=${DB_NAME}
        - DB_USER=${DB_USER}
        - DB_PWD=${DB_PWD}
        - DB_SERVER=${DB_SERVER}
        #adding server test
   ```
4. Configure the ENV variables.
   There are multiple options for setting the environment variables, but we used the .bashrc file locally for this exercise.

   ```bash
   vim ~/.bashrc
   source ~/.bashrc
   ```
   ```bash
   #ApplicationEnvVariables
    #MySQLDB
    export DB_NAME=mysql-db
    export DB_USER=unicorn
    export DB_PWD=unicornpassword
    export DB_SERVER=mysql-db
    export MYSQL_ROOT_PASSWORD=password
    
    #phpmyadmin    
    export PMA_PORT=3306
    export PMA_HOST=mysql-db
   ```
   <img src="" width=800 />
   

## Run Application on Digital Ocean with Docker Compose
1. Create a Droplet on Digital Ocean:
   a) Select **Create Droplet** from the Digital Ocean dashboard. <br>

      <img src="https://github.com/lala-la-flaca/deploy-java-app-digitalocean/blob/main/resources/Img/create%20a%20droplet%201.png?raw=true" width=800 />
    
   b) Select a region: Select the region closest to your location.<be>

      <img src="https://github.com/lala-la-flaca/deploy-java-app-digitalocean/blob/main/resources/Img/Droplet%20region.png?raw=true" width=800 />

   c) Select an image: Select the Ubuntu image.<br>

      <img src="https://github.com/lala-la-flaca/deploy-java-app-digitalocean/blob/main/resources/Img/CreatingDropletChooseImage.png?raw=true" width=800 />
     
     - Choose Size: <br>
       * Droplet type: Select Basic.<br>
       * CPU options: Select Regular SSD and the 8GB option.<br>

   d) Select the SSH key  as the Authentication Method. <br>
    - Generate a New SSH Key (if required): If you do not have an existing SSH key, follow DigitalOceans's guide to create a new pair.
    - Use an existing SSH Key: if you already have a public SSK key pair, navigate to the .ssh folder in your local directory. Copy the public key and paste it into the appropriate field in DigitalOCean's interface.<br>

      <img src="https://github.com/lala-la-flaca/deploy-java-app-digitalocean/blob/main/resources/Img/DropletSSH.png?raw=true" width=800 />

   e) Select **Create Droplet**

11. Configuring Firewall on Digital Ocean: Following security best practices, configure the firewall's inbound and outbound rules. In this case, you allow inbound SSH access from your machine to the Droplet, Nexus port, Nexus client, myphpadmin port, and application port. restricting all other unnecessary connections.

12. Select the **Networking** option from the left panel, then choose **Firewall**.

   <img src="https://github.com/lala-la-flaca/deploy-java-app-digitalocean/blob/main/resources/Img/SettingUpFirewall.png?raw=true" width=800 />

11. Click on **Create Firewall**
  
12. Set the firewall rules for incoming traffic.<br>
   Allow SSH access from your machine by adding an inbound rule that allows traffic from the public IP address of your machine on port 22 and Nexus port 8081.

   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_7_docker_Nexus_Docker/blob/main/Img/Adding%20Firewall%20Rules%20to%20Droplet.PNG" width=800 />
   
11. SSH into the droplet to verify that everything works as expected.

   ```bash
   ssh root@
   ```
       
2. Update the package manager and install Docker.

   ```bash
   apt update
   apt net-tools
   snap install docker
   ```

   <img src="" width=800/>

3. Allow docker to access insecure registries as docker was installed with snap with must modify the daemon.json file located in /var/snap/docker/current/config. IF the file does not exist, then you must create it.
  
   Adding insecure registries to file:
   ```bash
       {
        "log-level": "error",        
        "insecure-registries" : ["157.230.56.153:8084"]
      }   
   ```
    ```bash
   cd /var/snap/docker/current/config
   vim daemon.json
   sudo snap restart docker
   ```
   
5. Add environment variables on the droplet.
6. Copy the Docker compose file from your local machine to the Droplet.
   ```bash
   scp docker-compose.yaml root@157.230.0.133:~/app
   ```
8. Log in to the Nexus repository to pull docker images.
   ```bash
   docker login 157.230.0.133/8084
   ```
10. After copying the docker-compose file to the droplet and setting  the environment variables, you can run the docker compose file.
    ```bash
    docker compose -f docker-compose.yaml up
    ```
12. To ensure that the application is running, verify the status of the containers.
    ```bash
    docker ps
    ```
14. Open a browser and navigate to the application.


## ❌ Troubleshooting & Fixes
🔴 Issue: The Application does not have access to MySQL and exits with code 1
```
bash
app-docker-exercises-1  | Caused by: org.springframework.beans.BeanInstantiationException: Failed to instantiate [java.sql.Connection]: Factory method 'getConnection' threw exception with message: Access denied for user 'unicorn'@'%' to database 'team-member-projects'
```
Even when the official documentation says that the MYSQL_DATABASE is optional and if we add a user and password, this user is granted superuser access, the database was not created using the team-member-project. I had to use the same name as the server-db; the user only had access to the mysql-db.
<img src="" width=800 />

