# Noe Tovar-MBA
# Setup-Microsoft-SQL-Server-on-Ubuntu-20.04
Setup Microsoft SQL Server on Ubuntu 20.04
# Setup Microsoft SQL Server on Ubuntu 20.04
* 💻 Youtube video to install ubuntu 20.04
* 💻 Youtube video to install SQL Sever below
YouTube Setup Microsoft SQL Server on Ubuntu 20.04
 

 

## Update Ubuntu

```bash
sudo apt update
sudo apt upgrade -y
sudo reboot
```
###  Import Microsoft public repository GPG key for Ubuntu
```bash
sudo wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
```
### Register the Microsoft SQL Server Ubuntu repository for SQL Server 2019
```bash
sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/20.04/mssql-server-2019.list)"
```
### Install SQL Server
```bash
sudo apt update
sudo apt install -y mssql-server
```
### Configure SQL server and set SA password
```bash
sudo /opt/mssql/bin/mssql-conf setup
```
### Check SQL service is running
```bash
systemctl status mssql-server --no-pager
```
### Check the listening port for MSSQL server
```bash
sudo apt install net-tools
sudo netstat -tnlp | grep sqlservr
```
### Open up the firewall port 1433 to connect remotely
Allow ssh and enable firewall
```bash
sudo ufw allow 22
sudo ufw allow 1433
sudo ufw allow 1434
sudo ufw enable
```
## Install SQL Server command-line tools
### Import the public repository GPG keys.
```bash
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
```
### Register the Microsoft Ubuntu repository
```bash
curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
```
### Install tools and ODBC drivers
```bash
sudo apt update 
sudo apt install -y mssql-tools unixodbc-dev
```

### Add tools folder to PATH environment variable in a bash shell.
```bash
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
```
### Connect to your server
```bash
sqlcmd -S localhost -U SA -P 'Password'
```
If successful, you should get a command prompt: 1>.

### Attaching existing SQL Server Database
Copy yourdbname.mdf and yourdbname_log.ldf to the /var/opt/mssql/data/ directory. Set the owner and permissions to mssql.
```bash
sudo cp yourdbname* /var/opt/mssql/data/
sudo su
Chown mssql:mssql /var/opt/mssql/data/yourdbname*
chmod u=+rw,g=+rw,o=-rw  /var/opt/mssql/data/yourdbname*
```
Run sql on the master

```sql
USE master;

CREATE DATABASE yourdbname 
ON PRIMARY (FILENAME = '/var/opt/mssql/data/yourdbname.mdf'),
   (FILENAME = '/var/opt/mssql/data/yourdbname_log.ldf') 
FOR ATTACH
```
