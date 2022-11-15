<h1 align=center><strong><em>Deployment 5</em></strong></h1>

<div align=left>This repository will contain information related to the deployment of the URL-shortner flask application. The main objective of this deployment will be to work with a containerized application and deploy the container using ECS Fargate. Details can be found in the Deployment instructions or the documentation linked below.</div>

## Location of Documentation:
* Deployment Document: [click here](https://github.com/bmol5/kuralabs_deployment_5/blob/main/Documentation/Deployment5.pdf)

## Location of Dockerfile:
* Deployment Document: [click here](https://github.com/bmol5/kuralabs_deployment_5/blob/main/dockerf/dockerfile)

## Deployment Objectives:
``` diff
+ [x] Demonstrate your ability to deploy a containerized application.
+ [x] Gain experience with containerization and ECS Fargate
```

## Deployment Instructions:
* Link to instructions: [click here](https://github.com/kura-labs-org/kuralabs_deployment_5/blob/main/Deployment-5_Assignment.pdf)


## Code Used:

### Jenkins Installation:
```
#!/bin/bash

sudo apt update
sudo apt -y install openjdk-11-jre
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get -y install jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
```


### Docker Installation:
```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```


### Terraform Installation:
```
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
```

