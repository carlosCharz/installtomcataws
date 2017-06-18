# Install Tomcat 8.x in AWS EC2 Instance (Amazon Linux AMI)

## Software to be installed
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

## Tomcat Paths
```
cd  /usr/share/tomcat8
Edit the tomcat-users file: 
vim /usr/share/tomcat8/conf/tomcat-users.xml
Add this line according your desired configuration: (vi commands: "i" for insert mode, "ESC" key to escape the inserting mode, ":wq" for write an quit)
<user name="admin" password="YOURPASSWORD" roles="admin,manager,admin-gui,admin-script,manager-gui,manager-script,manager-jmx,manager-status" />
```

## Check Tomcat Installation
* fuser: to display the process id(PID) of every process using the specified files
* netstat: to list out all the network (socket) connections on a system
```
fuser -v -n tcp 8080
netstat -na | grep 8080
```

## Start Tomcat Service
```
service tomcat8 start
```

## Set Auto Start for Tomcat Service
```
sudo chkconfig --list tomcat8
```
