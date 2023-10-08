# docker-on-aws
# Docker and Apache Web Server Setup on AWS EC2

This project guides you through setting up Docker and launching an Apache web server on an Amazon Web Services (AWS) EC2 instance. By the end of this guide, you will have a functional Apache web server running inside a Docker container.

## Prerequisites

- AWS account with access to EC2 instances.
- SSH key pair for secure access to EC2 instance.
- Basic knowledge of AWS EC2 and Docker.

## Steps

### Step 1: Sign in to AWS Management Console

Log in to the AWS Management Console using your AWS credentials.

### Step 2: Launch an EC2 Instance

Create an EC2 instance with your desired specifications, e.g., Amazon Linux AMI, t2.micro. Be sure to associate your SSH key pair with the instance during launch.

### Step 3: SSH into the EC2 Instance

Use the private key of your SSH key pair to securely connect to the EC2 instance.

```bash
ssh -i <path-to-private-key> ec2-user@<instance-public-ip>
```
### Step 4: Update Package Manager and 
Update the package manager:
```
sudo yum update -y
``````
Install Docker for Linux Platform
``````
sudo yum install docker -y 
``````

### Step 5: Start the Docker Service and Enable it
Start the Docker service:

```
sudo service docker start
``````
Enable Docker to start on boot:
``````
sudo chkconfig docker on
``````
Check the status of the Docker service:
````
sudo service docker status
``````
Get information about Docker:
``````
docker info
``````
### Step 6: Pull the Docker Image
Pull a Docker image from a registry, for example:
``````
docker pull public.ecr.aws/docker/library/centos:latest
``````
### Step 7: Create a Dockerfile
Navigate to the directory where you want to create the Dockerfile, e.g., /opt/docker/.

``````
cd /opt
``````
``````
mkdir docker
``````
``````
cd docker
``````
### Create a Dockerfile using a text editor (e.g., vi) and add the following content:
make sure Dockerfile has an uppercase D
````
FROM public.ecr.aws/docker/library/centos:latest          
MAINTAINER QuantumCode
RUN cd /etc/yum.repos.d/
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
RUN yum -y update
RUN yum -y install httpd
COPY index.html /var/www/html/
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
EXPOSE 80
``````
save and exit the editor

### Step 8: Create the index.html File
Create the index.html file using a text editor (e.g., vi) and paste the content index.html file:

### Step 9: Build the Docker Image
Build the Docker image with the Dockerfile in the current directory (.) and tag it as "quantum_apache_image":

``````
docker build -t quantum_apache_image .
``````
### Step 10: List Docker Images
List the Docker images to verify that the "quantum_apache_image" has been successfully built:
``````
docker images
``````
### Step 11: Run the Docker Container
Run the Docker container in detached mode (-d) and map port 80 from the container to port 80 on the host:

``````
docker run -itd -p 80:80 quantum_apache_image
``````
### Step 11: Check Running Containers
List the running Docker containers to verify that the "quantum_apache_image" container is running:

``````
docker ps
``````
### Step 12: Verify Apache Server
To check if the Apache server is running and serving the content, open a web browser and enter the IP address of the EC2 instance.

You should be able to see the content you pasted in the index.html file displayed in the browser.

Congratulations! You have successfully set up an Apache web server inside a Docker container on an AWS EC2 instance.