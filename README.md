# CI/CD pipeline to deploy MEAN stack app
![dd_assignment_1](https://github.com/user-attachments/assets/0a8afeb6-6ccc-4970-a8a3-0453eb5119e4)

## Project Overview

This is a containerized full-stack MEAN application with Jenkins CI/CD.  

### Key Files

- [backend/Dockerfile](backend/Dockerfile) – Docker setup for the Node.js backend
- [frontend/Dockerfile](frontend/Dockerfile) – Docker setup for the Nginx-Angular frontend
- [frontend/nginx.conf](frontend/nginx.conf) – Custom Nginx config for frontend deployment
- [Jenkinsfile](./Jenkinsfile) - Contains jenkins pipeline script
- [docker-compose](./docker-compose) - docker-compose file to run frontend, backend, database containers together as a same service
  
  
### Project Structure

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


## Jenkins Configurations
To visualize and manage your CI/CD pipeline effectively, install the following recommended Jenkins plugins:

- **Pipeline: Stage View** – Visualize stages in your pipeline.

>  You can install these from **Jenkins Dashboard → Manage Jenkins → Plugins**.

### Jenkins Credentials

- Added **DockerHub credentials** (`Username with Password`) to Jenkins under `Manage Jenkins → Credentials → Global` for secure authentication.
- These credentials are used in the Jenkins pipeline to **push Docker images** to DockerHub without exposing sensitive data in the script.

<br/>

![jenkins_cred](https://github.com/user-attachments/assets/8b139e6a-8762-478c-91dc-3bb20744242e)

<br/>

- dockerhub token with read write access

<br/>

![docker_cred](https://github.com/user-attachments/assets/7c5b0b63-a6c9-48e4-8e94-56ece7502fad)


### Jenkins Pipeline

- Go to Jenkins Dashboard → New Item, choose Pipeline, and give it a meaningful name and description in the General section.

![image](https://github.com/user-attachments/assets/e85f5e32-7392-4102-998e-9bf71db79b6d)

<br/>

- In the Build Triggers section, select "GitHub hook trigger for GITScm polling" to automatically run the pipeline when a new commit is pushed.

![image](https://github.com/user-attachments/assets/bcf85ec5-1526-449a-b871-97e6d254c158)

<br/>

- In the Pipeline section, choose "Pipeline script from SCM", set SCM to Git, paste your GitHub repository URL, select the desired branch, and provide the path to your Jenkinsfile and then apply and save.

![image](https://github.com/user-attachments/assets/47b5b52b-0d49-46b3-8958-734cc7c8000e)





## GitHub Webhook Setup

- In your **repository settings**, go to **Webhooks** and add the **Payload URL** and set the content type to **application/json**.

![Webhook Step 1](https://github.com/user-attachments/assets/c1125e84-4d5d-474a-abd4-dd17e596be0f)
<br/>
![Webhook Step 2](https://github.com/user-attachments/assets/d99e5ac1-430e-4e3e-8c98-3921ed273e98)


- ✅ If you see a **green check mark**, the webhook is active.
- Any new commits pushed to the repo will automatically trigger the **Jenkins pipeline**.



## Nginx

- [frontend/nginx.conf](frontend/nginx.conf) – Custom Nginx config for frontend deployment
- Serves the Angular app on port 80 and uses Nginx reverse proxy to route /api/ requests to the backend, avoiding CORS issues by keeping all traffic under the same domain.

### Serves at Port 80

- Nginx listens on port 80, making the Angular frontend accessible via http://<server-ip> without needing to specify a port.

<pre lang="markdown">
server {
    listen 80;
    ...
}
</pre>


### API Reverse Proxy

- Forwards all /api/ requests from frontend to the backend container running on `http://backend:8081`.

<pre lang="markdown">
location /api/ {
    proxy_pass http://backend:8081/api/;
    ...
}
</pre>
  
## CI/CD pipeline

![dd_assignment_1](https://github.com/user-attachments/assets/0a8afeb6-6ccc-4970-a8a3-0453eb5119e4)


- As soon as a commit is pushed to the main branch, the configured GitHub webhook triggers the Jenkins pipeline automatically.

### docker images
- build images , list of images pic

### docker hub
- dockerhub images push and pics

### docker-compose
- docker compose , list of containers image




