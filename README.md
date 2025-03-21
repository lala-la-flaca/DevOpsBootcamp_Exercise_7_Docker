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
   
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/01%20Github%20mysql%20db.PNG" width=800 />
   
2. Pull the MySql image.
   
   ```bash
   docker pull mysql
   ```
   
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/01%202%20pulling%20image.png" width=800 />
   
3. Verify the downloaded image.
   
   ```bash
   docker images
   ```
   
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/01%20Github%20mysql%20img.png" width=800 />
   
4. Run MySQL container.
   
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
   
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/01%20Starting%20Container4%20with%20ports.png" width=800 />
   
5. Verify that the container is running.

   ```bash
   docker ps
   ```
   

## Start phpmyAdmin docker container
1. Open a browser and go to Docker Hub to find the official phpMyAdmin image.
   
   [DockerHub](https://hub.docker.com/_/phpmyadmin)

   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/02%20DockerHub%20phpmyadmin.PNG" width=800 />
   
2. Pull phpMyAdmin official image.

   ```bash
   docker pull phpmyadmin
   ```
   
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/02%20PullingMysqlGUI.PNG" width=800 />
   
3. Verify the downloaded image.

   ```bash
   docker images
   ```
   
4. Run phpMyAdmin container.

   ```bash
   docker run \
    --name phpmyadmin \
    -d \
    --link mysql-db:db \
    -p 8085:80 \
   phpmyadmin
   ```
   
5. Verify that the container is running.

   ```bash
    docker ps
    ```
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/02%20runningPhpMyAdmin%20container.PNG" width=800 />
   
6. Open a browser and go to phpMyAdmin.

    [phpMyAdmin](http://157.230.0.133:8085/)

    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/02%20phpmyadmin%20UP.png" width=800 />


## Configure Docker Compose to start Mysql & Phpmydmin together.
1. Create a YAML docker compose file in the project directory. Navigate to the root folder of the application, click on New, and file.

   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/Creating%20file.PNG" width=800 />
   
2. Define services and add a Volume to persist data from Mysql.

   ```bash
   version: '3'
    services:
      mysql-db:
        image: mysql
        ports:
          - 3306:3306
        environment:
          - MYSQL_ROOT_PASSWORD=password
          - MYSQL_DATABASE=mysql-db
          - MYSQL_USER=unicorn
          - MYSQL_PASSWORD=unicornpassword
    
        volumes:
          - mysql-db-data:/var/lib/mysql
          #- ./mysql-init-scripts:/docker-entrypoint-initdb.d
    
      phpmyadmin:
        image: phpmyadmin
        ports:
          - 8085:80
        restart: always
        environment:
          - PMA_HOST=mysql-db
          - PMA_PORT=3306
          - MYSQL_ROOT_PASSWORD=password
    
        depends_on:
          - "mysql-db"
    
    volumes:
      mysql-db-data:
        driver: local
   ```
   
3. Navigate to the root folder of the application and run Docker compose.

   ```bash
   docker compose -f docker-compose.yaml up
   ```
   
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/03%20Running%20Docker%20compose.PNG" width=800 />
   
4. Verify that both containers are running.

   ```bash
   docker ps
   ```
   
5. Run the Java application locally.
   
   ```bash
   gradle build
   java -jar docker-exercises-project-1.0-SNAPSHOT.jar
   ```

   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/01%20Starting%20application%20locally.png" width=800 />
   

## Dockerize Java Application
1. Create a Dockerfile in the root directory of the Java application.

   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/Creating%20file.PNG" width=800 />
      
2. Add the instructions to dockerize the Java application

   ```bash
    FROM openjdk:17-jdk-alpine    
    
    #creating /home/app directory in docker
    RUN mkdir /opt/app
    
    #copying only the content from app folder to the home/app in docker
    COPY ./build/libs/docker-exercises-project-1.0-SNAPSHOT.jar /opt/app
    
    # set default dir so that next commands executes in /home/app dir
    WORKDIR /opt/app
    
    #exposing port
    EXPOSE 8080
    
    # no need for /home/app/server.js because of WORKDIR
    CMD ["java","-jar","docker-exercises-project-1.0-SNAPSHOT.jar"]
       
   ```
   
3. Navigate to the project directory and build the .jar file for your application with gradle.

   ```bash
   gradle build
   ```
   
4. Build the docker image of the application.

   ```bash
   docker build -t docker-exercises:1.0 .
   ```
   
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/04%20building%20docker%20file.PNG" width=800 />


## Push the Docker Image to the Nexus repository
1. Open your browser and navigate to your Nexus Repository.
2. Log in to Nexus as an Admin user.
3. In the Nexus interface, go to settings, click on repositories, and create a Docker-hosted repository.
   
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_7_docker_Nexus/blob/main/Img/Create%20a%20docker%20Hosted%20Repo%20in%20NExus.png" width=800 />
   
4. Navigate to security, click on role, and create a role.

   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/DockerRole.png" width=800 />
  
5. Add the privileges to the role to access the docker hosted respository.
   
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/05%20adding%20privileges.png" width=800 />
   
6. Assign the role to the user.
   
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/userwithDockerRole.PNG" width=800 />
   
7. Navigate to the docker hosted repository and add the Nexus Docker adapter to allow docker clients.
    
    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/05%20Cretaing%20a%20Docker%20repository%20on%20nexus%20and%20adding%20connector.PNG" width=800 />
    
8. Add the Docker client port access to the firewall of the droplet where Nexus is hosted.
    
    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/05%20Opening%20the%20port%20for%20nexus%20connector.png" width=800 />
    
9. Add insecure registries to daemon.json file for port 8084
    
    ```bash
    cd /var/snap/docker/current/config
    vim daemon.json
    sudo snap restart docker    
    ```
    ```bash
    {       
        "insecure-registries" : ["157.230.56.153:8083","157.230.56.153:8084"]
    }   
     ```
    
    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/5%20adding%20insecure%20repository%20port%2084.PNG" width=800 />
    
10. Log in to Nexus from the command line.
  
    ```bash
    docker login http://157.230.56.153:8084
    ```
    
    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/5%20login%20to%20docker%20reposiotry.PNG" wodth=800 />
    
11. Tag the docker image with the appropriate respository name and version tag.
    
    ```bash
    docker tag docker-exercises:1.0 157.230.56.153:8084/docker-exercises:1.0
    ```
    
12. Push the image to the Nexus Docker registry.
    
    ```bash
    docker push docker-exercises:1.0 157.230.56.153:8084/docker-exercises:1.0
    ```
    
    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/5%20pushing%20docker%20images%20to%20the%20repo.png" width=800 />
    
13. Verify that the image is available in the Docker repository on Nexus.
    
    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/5%20Image%20available%20Repository.png" width=800 />


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
3. Configure the ENV variables.
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
 
4. Modify the docker-compose file to use Environment variables.

   ```bash
   version: '3'
    services:
    
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

<details><summary><strong>⚠️ Modify Application to point to the remote server</strong></summary>
Since the application was initially configured to run locally using localhost, update the code to replace localhost with the Droplet's IP address. After making this change, build a new Docker image and push it to the Nexus repository by following the previously outlined steps
</details>



   
## Run Application on Digital Ocean with Docker Compose
1. Create a Droplet on Digital Ocean:</br>
   a) Select **Create Droplet** from the Digital Ocean dashboard.
       <img src="https://github.com/lala-la-flaca/deploy-java-app-digitalocean/blob/main/resources/Img/create%20a%20droplet%201.png?raw=true" width=800 />
    
   b) Select a region: Select the region closest to your location.
       <img src="https://github.com/lala-la-flaca/deploy-java-app-digitalocean/blob/main/resources/Img/Droplet%20region.png?raw=true" width=800 />

   c) Select an image: Select the Ubuntu image.
       <img src="https://github.com/lala-la-flaca/deploy-java-app-digitalocean/blob/main/resources/Img/CreatingDropletChooseImage.png?raw=true" width=800 />
     
     - Choose Size: <br>
       * Droplet type: Select Basic.<br>
       * CPU options: Select Regular SSD and the 24GB option.<br>

   d) Select the SSH key  as the Authentication Method.
    - Generate a New SSH Key (if required): If you do not have an existing SSH key, follow DigitalOceans's guide to create a new pair.
    - Use an existing SSH Key: if you already have a public SSK key pair, navigate to the .ssh folder in your local directory. Copy the public key and paste it into the appropriate field in DigitalOCean's interface.

      <img src="https://github.com/lala-la-flaca/deploy-java-app-digitalocean/blob/main/resources/Img/DropletSSH.png?raw=true" width=800 />

   e) Select **Create Droplet**


2. Select the **Networking** option from the left panel, then choose **Firewall**.

    <img src="https://github.com/lala-la-flaca/deploy-java-app-digitalocean/blob/main/resources/Img/SettingUpFirewall.png?raw=true" width=800 />

3. Click on **Create Firewall**
  
4. Set the firewall rules for incoming traffic. Following security best practices, configure the firewall's inbound and outbound rules. In this case, you allow inbound SSH access from your machine to the Droplet, Nexus port, Nexus client, myphpadmin port, MySQL, and application port. restricting all other unnecessary connections.

    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/Firewall%20ports.PNG" width=800 />
   
5. SSH into the droplet to verify that everything works as expected.

   ```bash
   ssh root@157.230.0.133
   ```
       
6. Update the package manager and install Docker.

   ```bash
   apt update
   apt net-tools
   snap install docker
   ```

7. Allow Docker to access insecure registries as docker was installed with snap with must modify the daemon.json file located in /var/snap/docker/current/config. IF the file does not exist, then you must create it.
  
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
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/Insecure%20registries.PNG" width=800 />
   
8. Add environment variables on the droplet.

   ```bash
   vim ~/.bashrc
   source ~/.bashrc
   ```
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/Environment%20Variables%20Dgital%20ocean.PNG" width=500 />
   
9. Copy the Docker compose file from your local machine to the Droplet.
   
   ```bash
   scp docker-compose.yaml root@157.230.0.133:~/app
   ```
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/Dockercompose%20yaml%20available%20DigitalOcean.PNG" width=800 />
   
10. Log in to the Nexus repository to pull docker images.
   
   ```bash
   docker login HTTP://157.230.0.133:8084
   ```
   
11. After copying the docker-compose file to the droplet and setting  the environment variables, you can run the docker compose file.
    
    ```bash
    docker compose -f docker-compose.yaml up
    ```
    
    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/Docker%20compose%20up%20DigitalOcean.PNG" width=800 />
    
12. To ensure that the application is running, verify the status of the containers.
    
    ```bash
    docker ps
    ```
    
13. Open a browser and navigate to the application.

    [Java Application Running](HTTP://157.230.0.133:8080)
    
    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/Java%20application%20running.PNG" width=800 />
  

14. Access to phpMyAdmin
    
    [phpMyAdmin](HTTP://157.230.0.133:8085)
    
    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/Access%20to%20Myphpadmin%20from%20server.PNG" width=800 />


## ❌ Troubleshooting & Fixes

🔴 Issue: The Application does not have access to MySQL and exits with code 1

```
bash
app-docker-exercises-1  | Caused by: org.springframework.beans.BeanInstantiationException: Failed to instantiate [java.sql.Connection]: Factory method 'getConnection' threw exception with message: Access denied for user 'unicorn'@'%' to database 'team-member-projects'
```

Even though the official documentation states that MYSQL_DATABASE is optional and that providing a user and password grants the user superuser access, the database was not created when using team-member-project. Instead, I had to use the same name as server-db, as the user was only granted access to the mysql-db database.

<img src="https://github.com/lala-la-flaca/DevOpsBootcamp_Exercise_7_Docker/blob/main/Img/Official%20Documentation.png" width=800 />

1. Get container ID.
   
  ```bash
  docker ps
  ```

2. Access MySQL running container as the root user.
   
  ```bash
  docker exec -it bdf457d8f1b9  mysql -u root -p
  ```

3. Inside the Container, we checked if the team-member-project was created, which was not the case:

  ```bash
  mysql> SHOW databases;
  +--------------------+
  | Database           |
  +--------------------+
  | information_schema |
  | mysql              |
  | mysql-db           |
  | performance_schema |
  | sys                |
  +--------------------+
  ```
4. Checking permission for my user, as the database was not created therfore the user had no access to team-member-projects
  ```bash
  mysql> SHOW GRANTS FOR 'unicorn'@'%';
  +-------------------------------------------------------+
  | Grants for unicorn@%                                  |
  +-------------------------------------------------------+
  | GRANT USAGE ON *.* TO `unicorn`@`%`                   |
  | GRANT ALL PRIVILEGES ON `mysql-db`.* TO `unicorn`@`%` |
  +-------------------------------------------------------+
  2 rows in set (0.00 sec)
  
  ```
That's why the application was unable to access the team-member-projects, once we replaced the team-project-dabase for the mysql-db, the issue was fixed.





