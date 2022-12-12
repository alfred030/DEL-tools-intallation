## UBUNTU

```
sudo java --version
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:openjdk-r/ppa
sudo apt-get update
sudo apt-get install openjdk-8-jdk
sudo cd /opt
sudo wget https://www.atlassian.com/software/bamboo/downloads/binary/atlassian-bamboo-6.10.4.tar.gz
sudo tar -xvf atlassian-bamboo-6.10.4.tar.gz
sudo mv atlassian-bamboo-6.10.4 bamboo
sudo cd /opt/bamboo
sudo ls -l
sudo vim /opt/bamboo/atlassian-bamboo/WEB-INF/classes/bamboo-init.properties
bamboo.home=/home/bamboo/bamboo-home
sudo useradd -s /bin/bash bamboo
sudo chown bamboo: /opt/bamboo/atlassian-bamboo
```

```
sudo vim /etc/systemd/system/bamboo.service
```

Paste the following lines in the created bamboo.service file and then save and close the file.

```
[Unit]

Description = Atlassian Bamboo

After = syslog.target network.target

[Service]

Type = forking

User = bamboo

ExecStart = /opt/bamboo/atlassian-bamboo/bin/start-bamboo.sh

ExecStop = /opt/bamboo/atlassian-bamboo/bin/stop-bamboo.sh

SuccessExitStatus = 143

[Install]

WantedBy = multi-user.target
```

 ```
 sudo systemctl enable bamboo.service
 if [ -f /etc/systemd/system/*.wants/bamboo.service ]; then echo "On"; else echo "Off"; fi
 ```

```
sudo vim /etc/init.d/bamboo
```

Then, copy and paste the following script inside the bamboo file. After that, save and close the file.

```
#!/bin/sh

set -e
#### VARIABLES ####
# Name of app ( bamboo, Confluence, etc )
APP=bamboo

# Name of the user to run as
USER=bamboo

# Location of application's bin directory
BASE=/opt/bamboo
case "$1" in
    # Start command
    start)
        echo "Starting $APP"
        /bin/su - $USER -c "export BAMBOO_HOME=${BAMBOO_HOME}; $BASE/bin/startup.sh &> /dev/null"
        ;;
    
    # Stop command
    stop)
       echo "Stopping $APP"
       /bin/su - $USER -c "$BASE/bin/shutdown.sh &> /dev/null"
       echo "$APP stopped successfully"
       ;;

    # Restart command
    restart)
        $0 stop
        sleep 5
        $0 start
        ;;

    *)
        echo "Usage: /etc/init.d/$APP {start|restart|stop}"
        exit 1
        ;;
esac
exit 0
```



```
sudo chmod 755 /etc/init.d/bamboo
sudo update-rc.d bamboo defaults
sudo service bamboo start
sudo service bamboo stop
sudo service bamboo restart
```

Then, change the default port to which bamboo listens from 8085 to 80 by editing the value of Port in the server.xml file.

```
sudo vim /opt/bamboo/conf/server.xml
```

```
sudo service bamboo restart
```

Now, you can access the Bamboo initial installation setup screen by browsing the following URL.

```
http://<server_IP_address>
```


