# NGINX Basic Auth

### 1. Install tool 
- `apache2-utils` (Debian, Ubuntu) 
- or `httpd-tools` (RHEL/CentOS/Oracle Linux)\
Ex: `sudo apt-get install apache2-utils`

### 2. Create file
> sudo htpasswd -c /etc/nginx/.htpasswd user1\

Press `Enter` and type password for user` at the prompts
#### 2.1 Create additional user
> sudo htpasswd /etc/nginx/.htpasswd user2
#### 2.2 Check file 
> cat /etc/nginx/.htpasswd\
user1:$apr1$/woC1jnP$KAh0SsVn5qeSMjTtn0E9Q0

### 3. Configuring NGINX
Inside a location that you are going to protect
```
location /admin {
    auth_basic “Administrator’s Area”;
    auth_basic_user_file /etc/nginx/.htpasswd; 
    #...
}
```
#### 3.1 Limit access whole website but some areas
```
server {
    ...
    auth_basic           "Administrator’s Area";
    auth_basic_user_file /etc/nginx/.htpasswd; 

    location /public/ {
        auth_basic off;
    }
}
```

### 4. Combine with IP
- A user must be `both` authenticated and have a valid IP address
- A user must be `either` authenticated, or have a valid IP address
- Use satisfy `all` or `any` for above scenario

Source: https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/