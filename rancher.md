
```
sudo docker run -d --restart=unless-stopped -p 8080:8080 rancher/server
```


## Launching Rancher Server - Single Container - Bind Mount MySQL Volume

```
sudo docker run -d -v <host_vol>:/var/lib/mysql --restart=unless-stopped -p 8080:8080 rancher/server
```