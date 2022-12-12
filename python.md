## Install Python 3 Using apt

## UBUNTU

```
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.8
python --version
```


## CENTOS

```
sudo yum update -y
sudo yum install https://repo.ius.io/ius-release-el$(rpm -E '%{rhel}').rpm
sudo yum update -y
sudo yum install -y python3
python3 --version
```