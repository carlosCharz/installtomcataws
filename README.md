# Install Tomcat 8.x in AWS EC2 Instance (Amazon Linux AMI)

You will find as well the configuration of **Nginx** and **PostgreSQL**. Special thanks to @caden for the contribution!

## Software to install
* Java 8
* Tomcat 8.x

## Check YUM Updates
```
sudo su
yum list installed
yum update
```

## Install Java 8
```
yum install java-1.8.0
yum remove java-1.7.0-openjdk
```

## Install Tomcat 8.x
```
yum install tomcat8 tomcat8-webapps tomcat8-admin-webapps tomcat8-docs-webapp
```

## Start Tomcat Service
```
service tomcat8 start
```

## Tomcat Paths
1 Edit the tomcat-users file
```
cd  /usr/share/tomcat8
vim /usr/share/tomcat8/conf/tomcat-users.xml
```
2 Add this line according your desired configuration (vi commands: "i" for insert mode, "ESC" key to escape the inserting mode, ":wq" for write an quit)
```
<user name="admin" password="YOURPASSWORD" roles="admin,manager,admin-gui,admin-script,manager-gui,manager-script,manager-jmx,manager-status" />
```

## Check Tomcat Installation
* fuser: to display the process id(PID) of every process using the specified files
```
fuser -v -n tcp 8080
```
* netstat: to list out all the network (socket) connections on a system
```
netstat -na | grep 8080
```

## Set Auto Start for Tomcat Service
```
sudo chkconfig --list tomcat8
sudo chkconfig tomcat8 on
```

## Install Nginx
```
yum install nginx
```

## Add Nginx Domain Binding
```
vi /etc/nginx/conf.d/shenhe.org.conf
```

```
server {
    listen       80;
    listen       [::]:80;
    server_name  shenhe.org;
    root         /usr/share/nginx/html;
    location / {
        proxy_connect_timeout 300;
        proxy_send_timeout 300;
        proxy_read_timeout 300;
        proxy_pass http://localhost:8080;
        }
}
```
You can add more bindings. It’s similar.
If you just want nginx to handle static files (like images, javascript, css etc), you just do it like this:
```
server {
    listen       80;
    listen       [::]:80;
    server_name  static.shenhe.org;
    root         /usr/share/nginx/html;
}
```
## Start Nginx
```
service nginx start
```
## Set Auto Star for Nginx Service
```
sudo chkconfig --list nginx
sudo chkconfig nginx on
```

## Install PostgreSQL
```
sudo yum install postgresql postgresql-server
```

## Initialize postgresql DB
```
sudo service postgresql initdb
```

## Start PostgreSQL
```
sudo service postgresql start
```

## Login in with postgres
```
sudo -u postgres psql
```

## Change Password
```
\password postgres
```

## Quit
```
\q
```
## Enable Remote Access
```
sudo vi /var/lib/pgsql9/data/pg_hba.conf
```
Add you IP to trust IP
```
host all all 125.69.29.0/24     trust
```
Also change this line from
```
host    all             all             127.0.0.1/32            ident
```
To
```
host    all             all             127.0.0.1/32            md5
```

```
sudo vi /var/lib/pgsql9/data/postgresql.conf
```
Change 
```
#listen_addresses = 'localhost'
```
To
```
listen_addresses = '*'
```
## Auto Start PostgreSQL
```
sudo chkconfig --list postgresql
sudo chkconfig postgresql on
```

## Add swap to EC2
```
sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=2048
sudo /sbin/mkswap /var/swap.1
sudo chmod 600 /var/swap.1
sudo /sbin/swapon /var/swap.1
```

## Auto mount Swap
To enable it by default after reboot, add this line to /etc/fstab:
```
/var/swap.1  swap        swap    defaults        0   0
```
