# CI/CD pipeline to deploy MEAN stack app
![dd_assignment_1](https://github.com/user-attachments/assets/0a8afeb6-6ccc-4970-a8a3-0453eb5119e4)


backend [Dockerfile](backend/Dockerfile)

frontend [Dockerfile](frontend/Dockerfile)

nginx conf file [nginx.conf](frontend/nginx.conf)

- projecct structure
- aws ec2 configs
- installations done through userdata
    - runs script as root user
- acess jenkins dashboard at port 8080
    - add dockerhub username & tocken to with id as dockerhub in jenkins credential 
    - install pipeline stage view plugin for clean pipline view
 
<pre lang="markdown"> 
📁 MEAN-app-docker-deploy 
├── 📁 backend 
│   ├── 📁 app 
│   ├── 📄 server.js 
│   └── 📄 Dockerfile
│   
├── 📁 frontend 
│   ├── 📁 src 
|   ├── 📄 nginx.conf
│   └── 📄 Dockerfile 
│   
├── 📄 Jenkinsfile
├── 📄 docker-compose.yml
└── 📄 README.md
</pre>

## System Configurations
> [!NOTE]
> AWS ec2 Ubuntu VM , 2vCPU 8GiB of memory and 20GiB of storage
### install git, java, docker, docker-compose and jenkins
All these installations and conifigurations are done using user data script which will be the first script to be run as root user after creating the VM.  
<pre>#!/bin/bash
apt update -y
apt upgrade -y
apt install git openjdk-17-jre-headless docker.io docker-compose -y
wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
apt-get update
apt-get install jenkins -y
usermod -aG docker jenkins
systemctl restart jenkins
systemctl enable jenkins</pre>

