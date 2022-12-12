## UBUNTU 
```
sudo apt update
sudo apt install snapd
sudo snap install terragrunt
```

## CENTOS

```
sudo yum install epel-release
sudo yum install snapd
sudo systemctl enable --now snapd.socket
sudo ln -s /var/lib/snapd/snap /snap
sudo snap install terragrunt
```