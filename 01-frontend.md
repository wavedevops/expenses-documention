# Frontend

* The frontend is the service in Expense to serve the web content over Nginx. This will have the web frame for the web application.

* This is a static content and to serve static content we need a web server. This server

* Developer has chosen Nginx as a web server and thus we will install Nginx Web Server.

Install Nginx

```shell
dnf install nginx -y 
```

Enable nginx

```shell
systemctl enable nginx
```
Start nginx

```shell
systemctl start nginx
```

**Try to access the service once over the browser and ensure you get some default content**

Remove the default content that web server is serving.

```shell
rm -rf /usr/share/nginx/html/*
```

Download the frontend content

```shell
curl -o /tmp/frontend.zip https://expense-builds.s3.us-east-1.amazonaws.com/expense-frontend-v2.zip
```
Extract the frontend content.

```shell
cd /usr/share/nginx/html
```

```shell
unzip /tmp/frontend.zip
```

**Try to access the nginx service once more over the browser and ensure you get expense content.**

Create Nginx Reverse Proxy Configuration.

```shell
vim /etc/nginx/default.d/expense.conf
```
Add the following content

```
proxy_http_version 1.1;

location /api/ { proxy_pass http://localhost:8080/; }

location /health {
  stub_status on;
  access_log off;
}
```

**Ensure you replace the localhost with the actual ip address of backend component server. Word localhost is just used to avoid the failures on the Nginx Server.**

Restart Nginx Service to load the changes of the configuration.

```shell
systemctl restart nginx
```