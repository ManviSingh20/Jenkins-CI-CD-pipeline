# Automated CI/CD Pipeline with Jenkins and Docker on AWS

## About

In this project, we will design and implement a comprehensive CI/CD pipeline to automate the process of building, testing, and deploying an application. By leveraging Jenkins, SonarQube, Docker, and AWS, this pipeline will facilitate a streamlined and efficient deployment workflow.
   
<img width="1247" alt="Screenshot 2024-09-17 at 7 33 41 PM" src="https://github.com/user-attachments/assets/6ed8934a-c3db-4c77-977d-d7b7a7db1cd7">

## Tools Used

1. Jenkins - Jenkins is an open-source automation server used to orchestrate and manage the CI/CD pipeline. It automates the build, test, and deployment processes, providing a robust framework for continuous integration and continuous delivery.

2. SonarQube - SonarQube is a code quality and security analysis tool that helps in identifying and fixing code issues. It performs static code analysis to detect bugs, vulnerabilities, and code smells, ensuring that the code adheres to best practices and maintains high quality.

3. Docker - Docker is a platform for developing, shipping, and running applications in isolated containers. It allows for the creation of a consistent environment across development, testing, and production stages by packaging the application and its dependencies into a Docker image.

4. AWS - Amazon Web Services (AWS) provides cloud infrastructure for hosting and deploying applications.

## Ports

1. Jenkins - 8080
2. SonarQube - 9000
3. Shopping-cart Application - 8070


## Pipeline Overview

1. Continuous Integration (CI) Pipeline: The CI pipeline focuses on automating the integration process of code changes. It includes the following stages:
   1. Git Checkout
   2. Compile
   3. Sonarqube Analysis
   4. Build Application
   5. Build & Push Docker Image
   6. Trigger CD Pipeline

2. Continuous Integration (CD) Pipeline: Following the successful execution of the CI pipeline, the CD pipeline is triggered automatically to deploy the application. It involves:
   1. Docker Deploy to Container

Once the deployment is complete, the application will be accessible through a web browser, ensuring that the latest version is live and functional.



## Requirements 

1. GitHub Repo Containing the Java Application Code
2. AWS Account
3. Jenkins Server
4. SonarQube
5. Docker
6. Maven



## Setup & Install Jenkins in AWS Ubuntu Instance

1. Create a t2.large Ubuntu Instance on AWS, this will serve Jenkins, Maven, SonarQube, Docker. Here, we are using a large instance rather that a free tier instance because we require a resource intensive instance. 

   <img width="661" alt="Screenshot 2024-09-18 at 4 01 29 AM" src="https://github.com/user-attachments/assets/77abce68-3c9e-4cc6-b6b6-162c83a4db4f">

2. Connect with the instance using EC2 Instance Connect.
3. Install Java and Jenkins using the following commands.

Install Java.
```
sudo su -
sudo apt update
sudo apt install openjdk-17-jre
```

Verify Java is Installed.
```
java -version
```

Now, proceed with installing Jenkins
```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

Note - Java is a prerequisite for running Jenkins because Jenkins is built on Java and operates as a Java-based application.

4.Copy the Public IP of the instance and paste it in your browser tab followed with :8080, to access Jenkins Server. 
```
<publicIP>:8080
```

5. By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.
   - EC2 > Instances
   - In the bottom tabs, click on Security
   - Security groups
   - Add inbound traffic rules as shown in the image (you can just allow TCP 8080 as well, in my case, I allowed ```All traffic```.

<img width="968" alt="Screenshot 2024-09-18 at 4 08 56 AM" src="https://github.com/user-attachments/assets/715e6ff4-4ad3-416b-94c0-ab9f032db35c">

6. Check if jenkins is running
   ```
   sudo systemctl status jenkins

   ```

7. Copy the path give on the web page and use it in the command:
   ```
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```
   It will provide with the secret key for administration access.

<img width="858" alt="Screenshot 2024-09-18 at 1 17 25 AM" src="https://github.com/user-attachments/assets/6cb13542-6ef9-4a8b-a861-ecdc180bcab7">

8. Install suggested plugins.
   
<img width="1013" alt="Screenshot 2024-09-19 at 1 32 26 AM" src="https://github.com/user-attachments/assets/b9723372-a3c8-43b9-a1a1-a86ff571f778">

9. Create user admin.
10. Save and continue.
11. Start using jenkins.



## Install Docker on our EC2 Instance

1. To install Docker on Ubuntu 24.04, we'll run the following three commands:
```
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
   
curl -fsSL https://get.docker.com -o get-docker.sh
   
sudo sh get-docker.sh
```

2. Group Docker user to grant the necessary permissions to your user without needing "sudo" for every Docker command.
```
sudo usermod -aG docker $USER
   
newgrp docker
```
3. Check Docker version and restart Docker.
```
docker version
   
sudo systemctl restart docker
```   

<img width="555" alt="Screenshot 2024-09-18 at 1 30 43 AM" src="https://github.com/user-attachments/assets/90b55989-d714-448e-9352-57fd4ca2bb58">



## Set Up SonarQube Server

1. Before creating a job on Jenkins, configure and install the following plugins, go to Dashboard > Manage Jenkins > Plugins > Available Plugins:

   ```Eclipse Temurin Installer```
   
   ```openJDK-native-plugin```
   
   ```SonarQube Scanner```

<img width="402" alt="Screenshot 2024-09-18 at 1 28 36 AM" src="https://github.com/user-attachments/assets/e09b4356-df0c-4b9e-980a-63bdf04c1e9b">

2. Add the Jenkins user to the Docker user group by using the following commands:
   ```
   sudo usermod -aG docker jenkins
   sudo systemctl restart jenkins
   ```

2. Create a container for SonarQube using command:
   
     ```
     sudo docker run -d -p 9000:9000 sonarqube:lts-community
     ```
      
<img width="716" alt="Screenshot 2024-09-18 at 1 31 07 AM" src="https://github.com/user-attachments/assets/aa737f37-5e2b-4cf5-b4fe-82c77b15eb54">

3. Paste the publicIP of the instance in the new browser window as ``` <publicIP>:9000 ```.
4. The SonarQube server will start
   
   - Use Username as: ``` admin ```
   - Use Password as: ``` admin ```

5. There will be a window to chance the password. Select the password of your own.

<img width="1389" alt="Screenshot 2024-09-18 at 1 33 38 AM" src="https://github.com/user-attachments/assets/4ca19318-23ae-4bb2-aeb9-25f05c541af0">



## Creating CI/CD Pipeline

1. Go back to jenkins and create a job.

2. The first job will be CI_Pipeline.

<img width="934" alt="Screenshot 2024-09-18 at 1 36 15 AM" src="https://github.com/user-attachments/assets/a00038fb-0fa8-4a29-8c78-78907df5cb86">

3. The second job will be CD_Pipeline.

<img width="882" alt="Screenshot 2024-09-19 at 2 57 27 AM" src="https://github.com/user-attachments/assets/36f48b5e-e9df-4dee-ae9f-c0ec6d25a130">


### CI_Pipeline

1. Enable the discard old builds while keepng max no. of builds to keep as 2.
2. Define the following tools on Jenkins. Go to Dashboard > Manage Jenkins > Tools. Set up the following:
   1. JDK > Name = jdk11 > Install automatically > Add Installer > Install from adoptium .net > Choose jdk-11.0.19+7.
   2. SonarQube Scanner Installations > Name = sonar-scanner > Install automatically > Version = SonarQube Scanner 4.8.0.2856
   3. Maven >  Name = maven > Install automatically > Version = 3.6.0
3. Click on apply & save.
4. Scroll down and write the script as follows. (Select the Hello World Template from the drop down arrow).
```
   pipeline {
    agent any
    
    tools{
        jdk 'jdk11'
        maven 'maven'
    }
    
    environment{
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/ManviSingh20/Jenkins-CI-CD-pipeline.git'
            }
        }
        
        stage('Compile') {
            steps {
                sh "mvn clean compile"
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                sh '''$SCANNER_HOME/bin/sonar-scanner \
                -Dsonar.url=http://65.2.137.41:9000/ \
                -Dsonar.login=squ_72473c72bb2aa2d87a1e8bb912088d460b6171bd \
                -Dsonar.projectName=shopping-cart \
                -Dsonar.java.binaries=. \
                -Dsonar.projectKey=shopping-cart
                '''
            }
        }
        
        stage('OWASP SCAN') {
            steps {
                dependencyCheck additionalArguments: ' --scan ./', odcInstallation: 'DP'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
        stage('Build Application') {
            steps {
                sh "mvn clean install -DskipTests=true"
            }
        }
        
        stage('Build & Push Docker Application') {
            steps {
                script{
                    withDockerRegistry(credentialsId: '7fcc7c8c-6396-4552-b38f-26ca905cd256', toolName: 'docker') {
                        sh  "docker build -t shopping:latest -f docker/Dockerfile ."
                            "docker tag shopping:latest manvisingh20/shopping:latest"
                            "docker push manvisingh20/shopping:latest"
                    }
                }
            }
        }
        
        stage('Trigger CD_Pipeline') {
            steps {
                build job: "CD_Pipeline" , wait: true
            }
        }
    }
}
```

5. The stages are as follows:
   1. Git Checkout - Use pipline syntax to write the script of copying the source code from github repo and write it in the script.
      
   <img width="1074" alt="Screenshot 2024-09-19 at 1 53 08 AM" src="https://github.com/user-attachments/assets/f8c0b025-db8a-46e5-97d7-fa2aefffb7cd">

   2. Compile
   3. SonarQube Analysis - Here, we will require to generate the login token which can be done by opening SonarQube Server > Administration    > Security > Users > Tokens > Name = 1 > Generate. Copy the token and use it in your script.
   4. OWASP SCAN
      - For this stage we have to download the following plugins:

         <img width="379" alt="Screenshot 2024-09-19 at 2 23 40 AM" src="https://github.com/user-attachments/assets/0e515e61-9a81-4512-a26c-b319c290db3c">

      - Define the following tools:
         - Dependency-Check installations > Name = DP > Install Automatically > Version = 6.5.1
         - Docker > Name = docker > Install Automatically > Download from docker.com > Version = Latest.
      - It has it's own format for script given below:
        ```
        dependencyCheck additionalArguments: '', odcInstallation: 'DP-check'
           dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
        ```
   5. Build Application

   6. Build & Push Docker Image - Generate script using Pipeline Syntax

      <img width="401" alt="Screenshot 2024-09-19 at 2 43 47 AM" src="https://github.com/user-attachments/assets/698b6ad8-e5d8-4662-afde-5932a31854db">

      <img width="1040" alt="Screenshot 2024-09-19 at 2 42 59 AM" src="https://github.com/user-attachments/assets/3835c54b-b2d0-457c-bcf0-220e44af723a">

      <img width="947" alt="Screenshot 2024-09-19 at 2 44 31 AM" src="https://github.com/user-attachments/assets/ec8cf780-f69d-4163-9adf-d9da92669a5b">

   7. Trigger CD_Pipeline.
      
As soon as the CI_Pipeline completes, it will automatically trigger the CD_Pipeline.

### CD_Pipeline

1. Enable the discard old builds while keepng max no. of builds to keep as 2, similar to CI_Pipeline.
2. Scroll down and write the script as follows. (Select the Hello World Template from the drop down arrow).

```
pipeline {
    agent any

    stages {
        stage('Docker Deploy to Container') {
            steps {
                withDockerRegistry(credentialsId: '7fcc7c8c-6396-4552-b38f-26ca905cd256', toolName: 'docker') {
                    sh  "docker run -d --name shopping-cart -p 8070:8070 manvisingh20/shopping:latest"
                        " "
                }
            }
        }
    }
}
```

The CD_Pipeline will have only 1 stage, ``` Docker Deploy to Container ```, containing the script having docker commands. Here, we can use the pipeline syntax to generate the script format. 

















