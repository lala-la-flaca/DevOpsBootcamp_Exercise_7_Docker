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
