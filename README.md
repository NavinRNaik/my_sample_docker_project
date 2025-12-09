# my_sample_docker_project
A Maven project and a Dockerfile

**Still src file to be added**

**A. Prerequisites on Jenkins Host (Master or Agent)**

**To enable Docker builds inside Jenkins:**

**Install Docker**

sudo apt-get update  
sudo apt install docker.io  
sudo usermod -aG docker jenkins  
sudo systemctl restart jenkins


Verify Jenkins User Can Run Docker
sudo su - jenkins  
docker ps
Install Required Plugins

Git Plugin

Pipeline Plugin

Credentials Binding Plugin

Docker Pipeline Plugin

Maven Integration Plugin

Configure Global Tools

Manage Jenkins → Global Tool Configuration → Add Maven (e.g. Maven_3_9)

Add JDK if needed

Add Credentials

GitHub Credential → SSH key or personal access token

DockerHub / ECR / GCR Registry Credential → username/password or token

B. GitHub → Jenkins Integration
Enable webhooks in GitHub repo settings.

In Jenkins, create a Multibranch Pipeline or a Pipeline job using the repo URL.

