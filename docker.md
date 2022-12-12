############ Ubuntu

Uninstall old versions

Older versions of Docker went by the names of docker, docker.io, or docker-engine. Uninstall any such older versions before attempting to install a new version:

 ```
 sudo apt-get remove docker docker-engine docker.io containerd runc

 ```

Install using the repository

```
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release 
``` 

Add Docker’s official GPG key:

```
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg 

```

Use the following command to set up the repository: 

``` 
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg 

```

Install Docker Engine 

```
sudo apt-get update 

```

``` 
sudo chmod a+r /etc/apt/keyrings/docker.gpg
sudo apt-get update

``` 


Install Docker Engine, containerd, and Docker Compose. 

``` 
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin 

``` 

Verify that the Docker Engine installation is successful by running the hello-world image: 

```
sudo docker run hello-world
```
