## How to Install Microsoft Defender in Linux Debian/Ubuntu Systems

```
sudo dnf instll yum-utils
```

## for Ubuntu only
```
sudo yum-config-manager --add-repo=https://packages.microsoft.com/config/fedora/33/prod.repo
```

```
sudo rpm --import http://packages.microsoft.com/keys/microsoft.asc
```

```
sudo apt install curl libplist-utils
```

### Then you can follow basically the same process:

```
curl -o microsoft.list https://packages.microsoft.com/config/ubuntu/20.04/prod.list
sudo mv ./microsoft.list /etc/apt/sources.list.d/microsoft-prod.list
sudo apt install gpg
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
sudo apt install apt-transport-https
sudo apt update
sudo apt install mdatp
```

```
mdatp scan full
```