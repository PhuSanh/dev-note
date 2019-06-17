# SSL Let's Encrypt Centos Nginx

## 1. Install Cerbot Let's Encrypt Client
Certbot Client make easier to install Let's Encrypt
- Install EPEL repository
```
yum install epel-release
```
- Install certbot-nginx
```
yum install certbot-nginx
```

## 2. Redirect from www to non-www
- We already have file `/etc/nginx/conf.d/test-website.com.conf`
- Create one more file for that domain
```
nano /etc/nginx/conf.d/test-website.com.redirect.conf
```
- Add these lines to file
```
server { server_name www.test-website.com; return 301 $scheme://test-website.com$request_uri; }
```
- Save and restart nginx
```
systemctl restart nginx
```
## 3. Get certification
```
sudo certbot --nginx -d test-website.com -d www.test-website.com
```
The first time, it will ask for email

If it ask method config HTTPS, choose `2`
#### DONE

## 4. Renew
The certification will expire in `90 days`
- Renew manually every 60 days `sudo certbot renew`
- Create cronjob auto renew
```
sudo crontab -e
```
- Run renew at 0h00 every day. Do nothing if certification still work
```
0 0 * * * /usr/bin/certbot renew >> /var/log/sslrenew.log
```

## * Additional
- Config `http2`\
File /etc/nginx/conf.d/test-website.com.conf\
Edit line
> listen 443 ssl http2;

- Update Diffe-Hellman
```
openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
```
```
nano /etc/nginx/conf.d/test-website.com.conf
```
Paste this line to `server` block, after ssl certbot
> ssl_dhparam /etc/ssl/certs/dhparam.pem;
- Redirect from http to https
Edit file `/etc/nginx/conf.d/test-website.com.conf`
> server {  
>   listen 80;  
>		server_name test-website.com;  
> 	rewrite ^(.*)  
> 	https://test-website.com$1 permanent;  
> }
