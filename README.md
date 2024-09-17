# Jenkins-CI-CD-pipeline

In this project, we will be creating 2 jobs:
1. CI Pipeline -
2. CD Pipeline -

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
4. Check for java version by using command - java -version. It will be java 11 but we installed java 8. since jenkinds require java 11 hence it is updated.
5. Start the server by using the following command -
   a. sudo systemctl start jenkins
   b. sudo systemctl enable jenkins
   c. sudo systemctl status jenkins
   Note - By default jenkins runs on port 8080.
6. Check whether we can access it or note. Go to instamces copy its public ip address and paste in the the new browser window, and add :8080 at last as it runs on port 8080 by default. it won't work.
7. Edit the inbound rules and set custom TCP on port 8080 from anywhere.
8. refresh the page. it is now working.
9. 
