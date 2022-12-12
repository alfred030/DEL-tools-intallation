## UBUNTU 

```
sudo apt update
sudo apt install nginx
sudo ufw app list
sudo ufw allow 'Nginx HTTP'
sudo ufw status
systemctl status nginx
```

## CENTOS

```
sudo yum install epel-release
sudo yum install nginx
sudo systemctl start nginx
sudo systemctl status nginx
sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload
```