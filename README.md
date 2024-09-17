# Jenkins-CI-CD-pipeline

In this project, we will be creating 2 jobs:
1. CI Pipeline - Building the application and performing all the code quality check, like security scanning on the source code. Then using Docker we will be creating the docker image.
2. CD Pipeline - Once the Ci pipeline completes successfully, the CD pipeline will be triggered automatically. once it starts, it will be deployed using docker container.

Once the application is successfully deployed we can access int in the browser.
   
<img width="1247" alt="Screenshot 2024-09-17 at 7 33 41 PM" src="https://github.com/user-attachments/assets/6ed8934a-c3db-4c77-977d-d7b7a7db1cd7">

## Setup & Install Jenkins in AWS Ubuntu Instance

What is Jenkins?
It is an automated CI/CD tools. It is a Java-based, open source automation server that helps automate the software development process. It's used to manage and control the various stages of software delivery, such as building, testing, packaging, and more.

Steps-

1. Login to AWS Management Console and create an EC2 oinstance with ubuntu AMI.
2. Connect with the instance using EC2 Instance Connect.
3. run the following commands:

   a. sudo apt update
   
   b. sudo apt install openjdk-8-jdk -y
   
   Note - Installing java in necesarry because jenkins is integrated with java, hence it won't work without it.
   
   c. Install jenkins by visiting [pkg.jenkins.io](https://pkg.jenkins.io/debian-stable/). run all the given commands step by step.
   
5. Check for java version by using command - java -version. It will be java 11 but we installed java 8. since jenkinds require java 11 hence it is updated.
6. Start the server by using the following command -
   a. sudo systemctl start jenkins
   b. sudo systemctl enable jenkins
   c. sudo systemctl status jenkins
   Note - By default jenkins runs on port 8080.
7. Check whether we can access it or note. Go to instamces copy its public ip address and paste in the the new browser window, and add :8080 at last as it runs on port 8080 by default. it won't work.
8. Edit the inbound rules and set custom TCP on port 8080 from anywhere.
9. refresh the page. it is now working.
10. Copy the path give on the web page and use it in the command sudo cat ____. it will provide with the secret key to for administration access.
11. install suggested plugins.
12. create user admin.
13. save and continue.
14. start using jenkins.

## Install Docker on our EC2 Instance

1. To install Docker in Ubuntu 24.04, we'll run the following three commands:
   a. for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
   c. curl -fsSL https://get.docker.com -o get-docker.sh
   d. sudo sh get-docker.sh
2. If it doesn't work then use the following commands:
   a. sudo usermod -aG docker $USER
   b. newgrp docker
   c. docker version
   d. sudo systemctl restart docker


## Starting With Job

Prerequisites - Before creating a job configure and install the following plugins:
1. Eclipse Temurin Installer.
2. openJDK-native-plugin
3. SonarQube Scanner - Will be used to perform code quality check analysis on our source code.
4. Create a container for SonarQube using command:
      docker run -d -p 9000:9000 sonarqube:lts-community
5. Go back to jenkins and create a job.













## Commands
1. ps -ef | grep jenkins - Start jenkins server.
2. 
















