# Backend
* Backend service is responsible for adding the given values to database. Backend service is written in NodeJS, Hence we need to install NodeJS.

* **Developer has chosen NodeJs, Check with developer which version of NodeJS is needed. Developer has set a context that it can work with NodeJS >20**

* Install NodeJS, By default NodeJS 16 is available, We would like to enable 20 version and install this.

**You can list modules by using dnf module list**

```shell
dnf module disable nodejs -y
```

```shell
dnf module enable nodejs:20 -y
```

```shell
dnf install nodejs -y
```

Configure the application.

Add application User
```shell
useradd expense
```

* User expense is a function / daemon user to run the application. Apart from that we don't use this user to login to server.

* Also, username expense has been picked because it more suits to our project name.

* We keep application in one standard location. This is a usual practice that runs in the organization.

Lets setup an app directory.

```shell
mkdir /app
```

Download the application code to created app directory.

```shell
curl -o /tmp/backend.zip https://expense-builds.s3.us-east-1.amazonaws.com/expense-backend-v2.zip
```

```shell
cd /app
```

```shell
unzip /tmp/backend.zip
```

* Every application is developed by development team will have some common softwares that they use as libraries. This application also have the same way of defined dependencies in the application configuration.

Lets download the dependencies.

```shell
cd /app
```

```shell
npm install
```

* We need to setup a new service in systemd so systemctl can manage this service

Setup SystemD Expense Backend Service

```shell
vim /etc/systemd/system/backend.service
```

```
[Unit]
Description = Backend Service

[Service]
User=expense
Environment=DB_HOST="<MYSQL-SERVER-IPADDRESS>"
ExecStart=/bin/node /app/index.js
SyslogIdentifier=backend

[Install]
WantedBy=multi-user.target
```

**NOTE: Ensure you replace <MYSQL-SERVER-IPADDRESS> with IP address**

Load the service.

```shell
systemctl daemon-reload
```

Start the service.
```shell
systemctl start backend
```

```shell
systemctl enable backend
```

* For this application to work fully functional we need to load schema to the Database.

* We need to load the schema. To load schema we need to install mysql client.

* To have it installed we can use

```shell
dnf install mysql -y
```

Load Schema

```shell
mysql -h <MYSQL-SERVER-IPADDRESS> -uroot -pExpenseApp@1 < /app/schema/backend.sql
```

Restart the service.
```shell
systemctl restart backend
```