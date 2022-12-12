## Installation of Jenkins 


## Installation for Ubuntu

```
sudo apt update
sudo apt install openjdk-11-jre openjdk-11-jdk  -y
java -version
```

```
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```


## Installation for CentOS

```
sudo yum update
sudo yum  install openjdk-11-jre -y
sudo yum  install openjdk-11-jdk -y
java -version
```


## Installation for Docker containers

```
docker network create jenkins
```

```
docker run \
  --name jenkins-docker \
  --rm \
  --detach \
  --privileged \
  --network jenkins \
  --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 \
  docker:dind \
  --storage-driver overlay2

```


## Installation for Docker-compose 

```
 # docker-compose.yaml
 version: '3.8'
 services:
   jenkins:
     image: jenkins/jenkins:lts
     privileged: true
     user: root
     ports:
      - 8080:8080
      - 50000:50000
    container_name: jenkins
    volumes:
      - /home/${myname}/jenkins_compose/jenkins_configuration:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
```