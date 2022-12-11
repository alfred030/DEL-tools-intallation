Install SonarQube on Ubuntu 

Before installing any packages on the Ubuntu server instance, it is recommended to update the system. Log in using the sudo user and run the following commands to update the system.

```
sudo apt-get update
sudo apt-get -y upgrade

```

Step 2- Install and configure PostgreSQL

Install the PostgreSQL repository.

```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -

```

Install the PostgreSQL database server by running:

```
sudo apt-get -y install postgresql postgresql-contrib

```

Start PostgreSQL server and enable it to start automatically at boot time by running:

```
sudo systemctl start postgresql
sudo systemctl enable postgresql

```

Change the password for the default PostgreSQL user.

```
sudo passwd postgres

```

Switch to the postgres user.
```
su - postgres

``` 

Create a new user by typing:

```
createuser sonar

```

Switch to the PostgreSQL shell.

```
psql

```

Set a password for the newly created user for SonarQube database.

```
ALTER USER sonar WITH ENCRYPTED password 'P@ssword';

```

Create a new database for PostgreSQL database by running:

```
CREATE DATABASE sonar OWNER sonar;

```

Exit from the psql shell:

```
\q

```

Switch back to the sudo user by running the exit command.

``` 
exit

```

Step 3: Download and configure SonarQube

Download the SonarQube installer files archive.

``` 
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-7.3.zip

``` 

Install unzip by running:

``` 
apt-get -y install unzip

```

Unzip the archive using the following command.

```
sudo unzip sonarqube-7.3.zip -d /opt 

``` 

Rename the directory:

``` 
sudo mv /opt/sonarqube-7.3 /opt/sonarqube 

```

Assign permissions to administrator user for directory /opt/sonarqube

``` 
sudo chown -R administrator:administrator /opt/sonarqube/

```

Open the SonarQube configuration file using your favorite text editor.

``` 
sudo nano /opt/sonarqube/conf/sonar.properties 

```

Find the following lines.

``` 
#sonar.jdbc.username=
   #sonar.jdbc.password= 
   
```

Uncomment and provide the PostgreSQL username and password of the database that we have created earlier. It should look like:

``` 
sonar.jdbc.username=sonar
sonar.jdbc.password=P@ssword

```

Next, find:

``` 
#sonar.jdbc.url=jdbc:postgresql://localhost/sonar 

```

Uncomment the line, save the file and exit from the editor.

Finally, tell SonarQube to run in server mode :

``` 
sonar.web.javaAdditionalOpts=-server

``` 

Step 4: Configure Systemd service

SonarQube can be started directly using the startup script provided in the installer package. As a matter of convenience, you should setup a Systemd unit file for SonarQube.

``` 
sudo nano /etc/systemd/system/sonar.service

``` 
Populate the file with: 

```
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking

ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop

User=root
Group=root
Restart=always

[Install]
WantedBy=multi-user.target

``` 

Start the application by running:

``` 
sudo systemctl start sonar

``` 

Enable the SonarQube service to automatically start at boot time. 

```
sudo systemctl enable sonar

```

To check if the service is running, run:

``` 
sudo systemctl status sonar 

```

Step 5 - Setup Nginx

Now that we've got the SonarQube server running, it's time to configure Nginx. Start by creating a new Nginx configuration file for the site:

``` 
sudo nano /etc/nginx/sites-enabled/sonarqube

``` 

Add this configuration so that Nginx will be able to route incoming traffic to SonarQube:

``` 
server{
    listen      9000;
    server_name sonarqube.developerinsider.co;

    access_log  /var/log/nginx/sonar.access.log;
    error_log   /var/log/nginx/sonar.error.log;

    proxy_buffers 16 64k;
    proxy_buffer_size 128k;

    location / {
        proxy_pass  http://127.0.0.1:9000;
        proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
        proxy_redirect off;

        proxy_set_header    Host            $host;
        proxy_set_header    X-Real-IP       $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header    X-Forwarded-Proto http;
    }
} 

  ```

Save and close the file. Next, make sure your configuration file has no syntax errors:

``` 
sudo nginx -t

```

If you see errors, fix them and run sudo nginx -t again. Once there are no errors, restart Nginx:

``` 
sudo service nginx restart

```

Step 6 — Setting Up SonarQube

`To set up your installation navigate to your server's domain name or public IP address: http://server_domain_name_or_IP.`




################################ Install using Docker.

``` 
sudo nano docker-compose.yml

version: "3"

services:

  SonarQube:

    image: SonarQube:community

    depends_on:

      - db

    environment:

      SONAR_JDBC_URL: jdbc:postgresql://db:5432/sonar

      SONAR_JDBC_USERNAME: sonartest

      SONAR_JDBC_PASSWORD: sonartest

    volumes:

      - SonarQube_data:/opt/SonarQube/data

      - SonarQube_extensions:/opt/SonarQube/extensions

      - SonarQube_logs:/opt/SonarQube/logs

    ports:

      - "9000:9000"

  db:

    image: postgres:12

    environment:

      POSTGRES_USER: sonartest

      POSTGRES_PASSWORD: sonartesrt

    volumes:

      - postgresql:/var/lib/postgresql

      - postgresql_data:/var/lib/postgresql/data

volumes:

  SonarQube_data:

  SonarQube_extensions:

  SonarQube_logs:

  postgresql:

  postgresql_data:

```