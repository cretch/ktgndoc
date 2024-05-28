cp
Demander sudo
Prendre les packages dans
Y:\Project\0283.2023.01.ProMonitor\Packages

Installation

## Install Java
Pretty straightforward:
`yum install java-latest-openjdk-headless.x86_64`

![[../SAP/Protocoles/attachments/Pasted image 20230327154328.png]]
You can check installed version with `java -version`

![[../SAP/Protocoles/attachments/Pasted image 20230327154648.png]] 
## Install ProMonitor


Untar the installer till you see the install.sh scripts

![[../SAP/Protocoles/attachments/Pasted image 20230328091315.png]]
Then you can move it to the server's filesystem. You can use sapdepot fileshare and the File Transfer app within Royal TS:

![[../SAP/Protocoles/attachments/Pasted image 20230328091724.png]]

Change the file owner if needed

The installation itself is pretty straightforward, just ./install.sh
You may encounter some questions with pretty obvious answers.
You can then start the system with `systemctl start promonitor`

systemctl enable promonitor

You can access system http://gdc06211:8888/

admin admin default
change password

Install drivers
#### SAP Java Connector (JCO)
Download 
ntar and keep aside the .tgz file (Windows)
- Use the .tgz file directly (Linux).
- Go into **Settings > Admin configuration > Upload > Upload JCO library**
#### SAP HANA driver
- This driver is mandatory to monitor SAP Hana databases
- The driver is a file named **ngdbc.jar** that you can find in this network share:  `\share\sapdepot\dist\hanapatch\rev60\SAP_HANA_CLIENT\client`
- Go into **Settings > Admin configuration > Upload > Upload JCO library*

Put licence received from agentil
tets conenctivity with first connexion

### Install mySQL
Login with root
```bash
#Install SQL
dnf install @mysql:8.0

#Create directories
mkdir /Data/backups
mkdir /Data/backups/main
mkdir /Data/backups/tenant
mkdir /Data/mysql

#Copy files over
cp -rf /var/lib/mysql/* /Data/mysql/
mv /var/lib/mysql /var/lib/mysql.backup

#Change permissions and ownership
chown -R mysql /Data/mysql/
chgrp -R mysql /Data/mysql/

```
ln -s /agentil/mysql /var/lib/mysql
```

#Startup mySQL
systemctl start mysqld
mysqld --initialize

! Deactivate logs

Check for initial password in **/var/log/mysql/mysqld.log**

login with `mysql -u root -p

Change the password and write it down in Crypt-o:
`ALTER USER root@'localhost' IDENTIFIED BY 'xxxxxxxxx';`

#### Database creation
We need one database for the configuration and another one per ProMonitor tenant.
In our case, that makes two (**not to be mistaken with SAP tenants**)
```mysql
CREATE DATABASE MAIN;
USE MAIN;

CREATE USER 'promonit'@'%' identified by 'LVJo6xkx795$';
GRANT ALL PRIVILEGES ON MAIN.* TO 'promonit'@'%';

CREATE DATABASE TENANT;
USE TENANT;

GRANT ALL PRIVILEGES ON TENANT.* TO 'promonit'@'%';

CREATE USER 'backup'@'localhost' IDENTIFIED BY '%=TXK>;XLoy2';
GRANT SELECT, PROCESS ON *.* TO 'backup'@'localhost';

#### Database configuration
Edit `/etc/my.cnf`
```bash
disable-log-bin

sort_buffer_size = 15M

[mysqldump]

password="%=TXK>;XLoy2"
```

Create `backup.sh` in the backup folder


```bash
#!/bin/sh

mysqldump -u backup --single-transaction --databases MAIN > /Data/backups/main/MAIN_`date +%Y-%m-%d`.sql

mysqldump -u backup --single-transaction --databases TENANT > /Data/backups/tenant/TENANT_`date +%Y-%m-%d`.sql

gzip /Data/backups/main/MAIN_`date +%Y-%m-%d`.sql

gzip /Data/backups/tenant/TENANT_`date +%Y-%m-%d`.sql

find  /Data/backups/tenant/ -name "*" -mtime +7 -delete

find  /Data/backups/main/ -name "*" -mtime +7 -delete
```
CHECK BACKUPSSSSSSSSSSSS

## Install Victoria Metrics
Directory and service file creation :
```bash
mkdir /Data/victoriametrics
mkdir /Data/victoriametrics-data
nano /etc/systemd/system/victoriametrics.service



