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
