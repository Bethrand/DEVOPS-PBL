# **Project 5: IMPLEMENT A CLIENT SERVER ARCHITECTURE USING MYSQL DBMS**

## STEP 1 — *Create and Configure Two Linux-based virtual servers EC2 Instances in AWS using ubuntu*

`mysql-server`

`mysql-client`

#### *Connect mysql-server to instance*

`ssh -i "ugosuji-ec2.pem" ubuntu@ec2-52-87-230-167.compute-1.amazonaws.com`

`sudo hostnamectl set-hostname mysql-server` - *to name/differenciate each instances* 

`exit`

`ssh -i "ugosuji-ec2.pem" ubuntu@ec2-52-87-230-167.compute-1.amazonaws.com`  - *Reconnect to confirm hostname change*

![mysql-server](./Images/mysql-server%20hostname%20change.PNG)

#### *Connect mysql-client to instance*

`ssh -i "ugosuji-ec2.pem" ubuntu@ec2-54-164-126-137.compute-1.amazonaws.com`

`sudo hostnamectl set-hostname mysql-client` - *to name/differenciate each instances* 

`exit`

`ssh -i "ugosuji-ec2.pem" ubuntu@ec2-54-164-126-137.compute-1.amazonaws.com`  - *Reconnect to confirm hostname change*

![mysql-client](./Images/mysql-client%20hostname%20change.PNG)

## STEP 2 - *On mysql server Linux Server install MYSQL server software*

`sudo apt update -y` 

`sudo apt upgrade -y`

`sudo apt install mysql-server -y`

`sudo systemctl enable mysql` - *To enable the server only*

## STEP 3 - *On mysql client Linux Server install MYSQL client software*

`sudo apt update -y`

`sudo apt upgrade -y`

`sudo apt install mysql-client -y`

## STEP 4 - *On mysql-server instance changed inbound rules to mysql port 3306 with mysql-client private ip address*

![mysql-server inboundrule](./Images/mysql-server%20inbound%20rule.PNG)

`sudo mysql_secure_installation` - *to secure our database*

##### *After running this command above to type New Password and to Re-enter new password: you ll see "..... Failed! Error SET PASSWORD has no significance for user 'root'@'localhost' as the authentication method used doesn't store authentication date in MySQL server. Please consider using ALTER USER instead if you want to change authentication params" shows up. To continue, Run this below to assign Password and follow prompt to set Password shown in line commands:*

`sudo mysql` - *open a terminal to run this on mysql-server to create user*

`mysql> create user 'remote_user'@'%' identified with mysql_native_password by 'Password1';` 

`mysql> create database project5_db;` 

`mysql> grant all on project5_db.* to 'remote_user'@'%' with grant option;` 

`mysql> FLUSH PRIVILEGES;` 

`exit`

## STEP 5 - *Configure mysql-server to allow connections from remote hosts*

`sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf`

![blind address](./Images/change%20blind%20address%20to%200.0.0.0.PNG)

`sudo systemctl restart mysql`

## STEP 6 - *From mysql-client connect to mysql-server database without using SSH. Use mysql utility to do this. 

`sudo mysql -u remote_user -h 172.31.54.200 -p`

`Password1`

`sudo mysql_secure_installation` 

## STEP 7 - *on mysql-client, Check for Successful Connection to a remote mysql-server and can perform SQL queries.*

`Show databases;`

![Show databases](./Images/Show%20Databases.PNG)

