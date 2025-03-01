# 🐳 Containers with Docker

## Description
This demo project is part of **Module 7: Containers with Docker** from the **Nana DevOps Bootcamp Exercise Guide**. It focuses on containerizing a **Java application with MySQL** and deploying it using **Docker Compose** on a remote server, using DigitalOcean Droplet.
<br />

## 🚀 Technologies Used

- <b>Docker and Docker Compose: Docker for containerization.</b>
- <b>Linux: Ubuntu for Server configuration and management.</b>
- <b>Java application: Java Application developed by Nana.</b>
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

## Prerequisites
Ensure that the Nexus repository from module 6 is up and running.

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
2. Pull the MySql image.
3. Verify the downloaded image.
4. Run MySQL container.
5. Verify that the container is running.

## Start phpmyAdmin docker container
1. Open a browser and go to Docker Hub to find the official phpMyAdmin image.
   
   [DockerHub](https://hub.docker.com/_/phpmyadmin)
   
   <img src="" width=800 />
   
3. Pull phpMyAdmin official image.
4. Verify the downloaded image.
5. Run phpMyAdmin container.
6. Verify that the container is running.
7. Open a browser and go to phpMyAdmin.

## Configure Docker Compose to start Mysql & Phpmydmin together.
1. Create a YAML docker compose file in the project directory.
2. Define services and add a Volume to persist data from Mysql.
3. Run Docker compose.
4. Verify that both containers are running.

## Dockerize Java Application
1. Create a Dockerfile in the root directory of the java application.
2. Navigate to the project directory and build the .jar file for your application with gradle.
3. Build the docker image of the application.

## Push the Docker Image to the Nexus repository
1. Open your browser and navigate to your Nexus Repository.
2. Log in to Nexus as an Admin user.
3. In the Nexus interface, go to settings, click on repositories, and create a Docker hosted repository.
4. Navigate to security, click on role, and create a role.
5. Add the privileges to the role to access the docker hosted respository.
6. Assign the role to the user.
7. Navigate to the docker hosted repository and add the Nexus Docker adapter to allow docker clients.
8. Log in to Nexus from the command line.
9. Tag the docker image with the appropriate respository name and version tag.
10. Push the image to the Nexus Docker registry.
11. Verify that the image is available in the Docker repository on Nexus.

## Add Java application to the Docker Compose file
1. Open the existing YAML docker compose file.
2. Edit the docker compose file and under the services section add a new service for your java application.
3. Configure the ENV variables.
   There are multiple options for setting the environment variables but for this exercise, we used the .bashrc file locally.

## Run Application on Digital Ocean with Docker Compose
1. Create a Droplet on digital ocean:
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

11. Configuring Firewall on Digital Ocean: Following security best practices, configure the firewall's inbound and outbound rules. In this case, you only allow inbound SSH access from your machine to the Droplet and access to the Nexus port. restricting all other unnecessary connections.

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

3. Allow docker to access insecure registries.
4. Add environment variables on the droplet.
5. Copy the Docker compose file from your local machine to the Droplet.
6. Log in to the Nexus repository to pull docker images.
7. After copying the docker-compose file to the droplet and setting  the environment variables, you can run the docker compose file.
8. To ensure that the application is running, verify the status of the containers.
9. Open a browser and navigate to the application.


## ❌ Troubleshooting & Fixes
🔴 Issue: The Application does not have access to MySQL and exits with code 1
```
bash
app-docker-exercises-1  | Caused by: org.springframework.beans.BeanInstantiationException: Failed to instantiate [java.sql.Connection]: Factory method 'getConnection' threw exception with message: Access denied for user 'unicorn'@'%' to database 'team-member-projects'
```
