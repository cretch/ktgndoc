- [[#Foreword|Foreword]]
	- [[#Foreword#Servers information|Servers information]]
		- [[#Servers information#Prerequisites|Prerequisites]]
		- [[#Servers information#Ports opening|Ports opening]]
		- [[#Servers information#Users and permissions|Users and permissions]]
- [[#Installation|Installation]]
	- [[#Installation#Install Java|Install Java]]
	- [[#Installation#Install ProMonitor|Install ProMonitor]]
		- [[#Install ProMonitor#Install drivers|Install drivers]]
			- [[#Install drivers#SAP Java Connector (JCO)|SAP Java Connector (JCO)]]
			- [[#Install drivers#SAP HANA driver|SAP HANA driver]]
	- [[#Installation#Install ProMonitor Cockpit|Install ProMonitor Cockpit]]
		- [[#Install ProMonitor Cockpit#Install mySQL|Install mySQL]]
			- [[#Install mySQL#Database creation|Database creation]]
			- [[#Install mySQL#Database configuration|Database configuration]]
	- [[#Installation#Install Victoria Metrics|Install Victoria Metrics]]

# Foreword

ProMonitor is a monitoring system for SAP. Cockpit is its companion app, to trigger notifications and/or modifiy alerts. The provider is Agentil.

Documentation from provider can be found [here](https://wiki.agentil-software.com/doku.php?id=start)
Download package can be found [here](https://wiki.agentil-software.com/doku.php?id=products:promonitor:6.8:privatereleasenote) but you must ask Agentil for an account.


> [!WARNING] ProMonitor Cockpit
> Since it's still under development, you msut contact Agentil to get the executable. So, please save the download under the Team folder


## Servers information

We've got two RedHat servers for the production. Dev is not intended to be a failover so it shouldn't have active alarms outside tests.
- GDC06211 - Production
- GDC06212 - Development

### Prerequisites
- [x] Linux CentOS/SUSE/RedHat (7 or above)
- [x] systemctl must be available for a smooth installation
- [x] Java OpenJDK 11 or above **(64 bits)** due to licensing costs
- [x] SAP JCO drivers for SAP NetWeaver systems
- [x] SAP HANA drivers for HANA systems
### Ports opening
>[!todo] 
>Redo the table of ports

Since we already had an existing ProMonitor installation, we can reuse the same rules.
### Users and permissions
>[!todo] 
>Talk about its-nimsrv roles in each system

> [!warning]
> Root access is needed on the system!

# Installation

## Install Java
Pretty straightforward:
`yum install java-latest-openjdk-headless.x86_64`

![[../SAP/Protocoles/attachments/Pasted image 20230327154328.png]]
You can check installed version with `java -version`

![[../SAP/Protocoles/attachments/Pasted image 20230327154648.png]] 
## Install ProMonitor

Before to installing Redpeaks, open install.sh script and update the pmDir variable

`pmDir="/opt"`

Then, open promonitor.service and set the same folder on the following lines :

`WorkingDirectory=/opt/Pro.Monitor/`
`ExecStart=/opt/Pro.Monitor/bin/startup.sh`


``` bash
mkdir /Data/downloads
chmod 777 downloads
```

Untar the installer till you see the install.sh scripts
![[../SAP/Protocoles/attachments/Pasted image 20230328091315.png]]
Then you can move it to the server's filesystem. You can use sapdepot fileshare and the File Transfer app within Royal TS:
![[../SAP/Protocoles/attachments/Pasted image 20230328091724.png]]
The installation itself is pretty straightforward, just ./install.sh
You may encounter some questions with pretty obvious answers.
You can then start the system with `systemctl start Promonitor`


> [!WARNING]
> Give URL to ressources


### Install drivers
You may need to install two special drivers, depending on your usecase.
This is done inside ProMonitor core
#### SAP Java Connector (JCO)
> [!WARNING]
> You must take 64bit-x86* version

Please download the JCO from [SAP website]([https://service.sap.com/connectors](https://service.sap.com/connectors))

- Untar and keep aside the .tgz file (Windows)
- Use the .tgz file directly (Linux).
- Go into **Settings > Admin configuration > Upload > Upload JCO library**

#### SAP HANA driver
- This driver is mandatory to monitor SAP Hana databases
- The driver is a file named **ngdbc.jar** that you can find in this network share:  `\share\sapdepot\dist\hanapatch\rev60\SAP_HANA_CLIENT\client`
- Go into **Settings > Admin configuration > Upload > Upload JCO library**
## Install ProMonitor Cockpit

Before to installing Redpeaks Cockpit, open install.sh script and update the pmDir variable

`pmDir="/opt"`

Then, open promonitor-cockpit.service and set the same folder on the following lines :

`WorkingDirectory=/opt/Pro.Monitor-Cockpit/`
`ExecStart=/opt/Pro.Monitor-Cockpit/bin/startup.sh`

Same process as ProMonitor Core except you should have a subfodler named cockpit in Downloads. Boot up the service with `systemctl start cockpit`.



### Install mySQL
Login with root
```bash
#Install SQL
dnf install @mysql:8.0

systemctl enable mysqld
systemctl start mysqld

#Create directories
mkdir /Data/backups
mkdir /Data/backups/main
mkdir /Data/backups/tenant
mkdir /Data/mysql

systemctl stop mysqld

cp -rf /var/lib/mysql/* /Data/mysql
mv /var/lib/mysql /var/lib/mysql.backup
chown -R mysql /Data/mysql
chgrp -R mysql /Data/mysql
ln -s /Data/mysql /var/lib/mysql

#Startup mySQL
systemctl start mysqld
mysqld --initialize
```

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
```
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

mysqldump -u root --single-transaction --databases MAIN > /Data/backups/main/MAIN_`date +%Y-%m-%d`.sql

mysqldump -u root --single-transaction --databases TENANT > /Data/backups/tenant/TENANT_`date +%Y-%m-%d`.sql

gzip /Data/backups/main/MAIN_`date +%Y-%m-%d`.sql

gzip /Data/backups/tenant/TENANT_`date +%Y-%m-%d`.sql

find  /Data/backups/tenant/ -name "*" -mtime +7 -delete

find  /Data/backups/main/ -name "*" -mtime +7 -delete
```

Set it up to run 
```bash
crontab -e
0 22 * * * /Data/backups/do_backups.sh
```

## Install Victoria Metrics
Directory and service file creation :
```bash
mkdir /Data/victoriametrics
mkdir /Data/victoriametrics-data
nano /etc/systemd/system/victoriametrics.service
```

Copy this

```bash
[Unit]
Description=Victoria Metrics
After=network.target

[Service]
Type=simple
StartLimitBurst=5
StartLimitInterval=0
Restart=on-failure

RestartSec=1
PIDFile=/Data/victoriametrics/victoriametrics.pid
ExecStart=/Data/victoriametrics/victoria-metrics-prod -storageDataPath /Data/victoriametrics-data -retentionPeriod 6 -graphiteListenAddr=:2003 -dedup.minScrapeInterval=1ms

ExecStop=/bin/kill -s SIGTERM $MAINPID

[Install]

WantedBy=multi-user.target
```

Then you should create the configuration file for the service : 

```bash
chmod 755 /etc/systemd/system/victoriametrics.service
mkdir /etc/systemd/system/victoriametrics.service.d
nano /etc/systemd/system/victoriametrics.service.d/ulimit.conf
```
Copy-paste this
```bash
[Service]

LimitNOFILE=32000
LimitNPROC=32000
```
### The script itself
We dl the script on github [https://github.com/VictoriaMetrics/VictoriaMetrics/](https://github.com/VictoriaMetrics/VictoriaMetrics/)

[victoria-metrics-linux-amd64-v1.85.1.tar.gz](https://github.com/VictoriaMetrics/VictoriaMetrics/releases/download/v1.85.1/victoria-metrics-linux-amd64-v1.85.1.tar.gz)

transfer to /Data/downloads


```bash
tar -zxvf victoria-metrics-amd64-v1.56.0.tar.gz
cp -R victoria-metrics-prod /Data/victoriametrics
systemctl enable victoriametrics
systemctl start victoriametrics
```


> [!todo]
> Backup
> SSO

# Configuration

> [!TODO] 
> We must complete this part, with howtos for some details

For initialization, we can take the export of the PoC
## Pro Monitor
Connect with admin > Admin configuration

LDAP/AD
![[../SAP/Protocoles/attachments/Pasted image 20230329110051.png]]
Mail
![[../SAP/Protocoles/attachments/Pasted image 20230329110110.png]]

### SAP user autorizations

In order to monitor an ABAP system, a dedicated user must be created in SAP and registered in Pro.Monitor. All the accesses performed in the system will be made on the behalf of this user

You can download a predefined authorization profile from : 
`Admin menu→Admin configuration→Upload/Download→Download SAP user profile`

Create the user in SAP via **SU01** transaction and associate it with the uploaded profile:

It must be a communication user

Following default settings must be used :

Its-nimsrv, has to update autorisations…

Upload the file in PFCG, it creates temp role Z_AGENTIL_SOFTWARE_PROMONITOR
Enter in  modification, generate auth, it creates profile.
Then check in its-nimsrv user for the correct role, mostly its ZA_U_XX_NIM_00
Copy profile to role from within autorisations

## ProMonitor Cockpit
First, connect to the webgui and initiate the admin account.
Then go to **Administration > DB connections** and create MAIN and TENANT db connections.

You must initialize the primary first!
Then you can create the Victoria metrics and enable it for internal monitoring.
You have to create all needed users in Tenant “user” panel. Beware of capitalism!

![[../SAP/Protocoles/attachments/Pasted image 20230329105905.png]]
To activate SSO, you have to login with a TENANT user, not the admin.
For the config of orgs, etc.. you must export from PM panel a JSON file, then import into cockpit.