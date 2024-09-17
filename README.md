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

1. Login to AWS Management Console and create an EC2 instance with ubuntu AMI.
2. Connect with the instance using EC2 Instance Connect.
3. run the following commands:

   a. sudo apt update
   
   b. sudo apt install openjdk-8-jdk -y
   
   c. Install jenkins by visiting [pkg.jenkins.io](https://pkg.jenkins.io/debian-stable/). run all the given commands step by step.

   <img width="1310" alt="Screenshot 2024-09-18 at 1 08 32 AM" src="https://github.com/user-attachments/assets/aa557e97-e5a9-4299-b335-df0e23ebec89">

   Note - Java is a prerequisite for running Jenkins because Jenkins is built on Java and operates as a Java-based application.
   
5. Check for java version by using command - java -version. It will be java 17 but we installed java 8. This is because Jenkins require java 11 hence it is updated in the later steps provided in the documentation.
6. Start the server by using the following command -
   a. sudo systemctl start jenkins
   b. sudo systemctl enable jenkins
   c. sudo systemctl status jenkins
   Note - By default jenkins runs on port 8080.
7. Check whether we can access it or not. Go to instance copy its public ip address and paste in the the new browser window, and add :8080 at last as it runs on port 8080 by default. It won't work at first as we haven't edited the inbound rules.
8. Edit the inbound rules and set custom TCP on port 8080 from anywhere.
9. Refresh the page. It is now working.
10. Copy the path give on the web page and use it in the command sudo cat ____. it will provide with the secret key for administration access.

<img width="858" alt="Screenshot 2024-09-18 at 1 17 25 AM" src="https://github.com/user-attachments/assets/6cb13542-6ef9-4a8b-a861-ecdc180bcab7">
11. Install suggested plugins.
12. Create user admin.
13. Save and continue.
14. Start using jenkins.

## Install Docker on our EC2 Instance

1. To install Docker on Ubuntu 24.04, we'll run the following three commands:
```
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
   
curl -fsSL https://get.docker.com -o get-docker.sh
   
sudo sh get-docker.sh
```

3. If it doesn't work then use the following commands:
```
sudo usermod -aG docker $USER
   
newgrp docker
   
docker version
   
sudo systemctl restart docker
```   
<img width="555" alt="Screenshot 2024-09-18 at 1 30 43 AM" src="https://github.com/user-attachments/assets/90b55989-d714-448e-9352-57fd4ca2bb58">



## Starting With Job

Prerequisites - Before creating a job configure and install the following plugins:

```Eclipse Temurin Installer.```

```openJDK-native-plugin```

```SonarQube Scanner``` - Will be used to perform code quality check analysis on our source code.

<img width="402" alt="Screenshot 2024-09-18 at 1 28 36 AM" src="https://github.com/user-attachments/assets/e09b4356-df0c-4b9e-980a-63bdf04c1e9b">

Create a container for SonarQube using command:
      docker run -d -p 9000:9000 sonarqube:lts-community
      
<img width="716" alt="Screenshot 2024-09-18 at 1 31 07 AM" src="https://github.com/user-attachments/assets/aa737f37-5e2b-4cf5-b4fe-82c77b15eb54">

This will start the sonarqube server.
Use Username as: ```admin```
use Password as: ```admin```

Next, there will be a window to chance the password. Select the password of your own.

<img width="1389" alt="Screenshot 2024-09-18 at 1 33 38 AM" src="https://github.com/user-attachments/assets/4ca19318-23ae-4bb2-aeb9-25f05c541af0">


Go back to jenkins and create a job.



<img width="402" alt="Screenshot 2024-09-18 at 1 28 36 AM" src="https://github.com/user-attachments/assets/e09b4356-df0c-4b9e-980a-63bdf04c1e9b">







## Commands
1. ps -ef | grep jenkins - Start jenkins server.
2. 
















