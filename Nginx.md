## Các lệnh thường dùng trên môi trường linux với nginx
Kill port 80 in use
```
sudo lsof -t -i tcp:80 -s tcp:listen | sudo xargs kill
```
Liệt kê port 80
```
sudo lsof -i -P -n | grep LISTEN
```
```
Xóa toàn bộ nginx
sudo apt-get remove nginx nginx-common
sudo apt-get purge nginx nginx-common
```
```sh
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl start nginx
sudo systemctl status nginx
sudo systemctl reload nginx
curl ifconfig.me => Kiểm tra IPWAN
sudo nginx -s reload
nginx -t
sudo tail -f /var/log/nginx/error.log
```
Mặc định nginx sẽ được khởi động khi reboot server, để disable dùng lệnh
```
sudo systemctl disable nginx
```
Ngược lại muốn nginx khởi động khi reboot server ta dùng lệnh
```
sudo systemctl enable nginx
```
Cài đặt nginx
```
sudo apt update
sudo apt install nginx
```
Kiểm tra firewall xem đã mã port 80/443 chưa
```
sudo ufw app list
```
Kết quả:
```
Output 
Available applications: 
Nginx Full 
Nginx HTTP 
Nginx HTTPS 
OpenSSH
Open http port
```
```sh
sudo ufw allow 'Nginx HTTP'

sudo ufw status
```
Kết quả
```
Output
Status: active
To Action From
-- ------ ----
OpenSSH ALLOW Anywhere 
Nginx HTTP ALLOW Anywhere 
OpenSSH (v6) ALLOW Anywhere (v6) 
Nginx HTTP (v6) ALLOW Anywhere (v6)
```
```
systemctl status nginx
```
```
Output
● nginx.service - A high performance web server and a reverse proxy server
Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
Active: active (running) since Fri 2020-04-20 16:08:19 UTC; 3 days ago
Docs: man:nginx(8)
Main PID: 2369 (nginx)
Tasks: 2 (limit: 1153)
Memory: 3.5M
CGroup: /system.slice/nginx.service
├─2369 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
└─2380 nginx: worker process
```
## Triển khai nhiều web trên cùng 1 host
```sh
sudo mkdir -p /var/www/domain_one.com/html
sudo mkdir -p /var/www/domain_two.com/html

sudo chown -R $USER:$USER /var/www/domain_one.com/html
sudo chown -R $USER:$USER /var/www/domain_two.com/html

echo $USER 

sudo chmod -R 755 /var/www/domain_one.com
sudo chmod -R 755 /var/www/domain_two.com

sudo chmod -R 777 /var/www/domain_one.com/public/wp-content

sudo nano /var/www/domain_one.com/html/index.html
sudo nano /var/www/domain_two.com/html/index.html
```
Ví dụ file index.html:
```html
<html>
<head>
<title>Welcome to domain one!</title>
</head>
<body>
<h1>Success! The your_domain server block is working!</h1>
</body>
</html>
```
## Install
```sh
sudo nano /etc/nginx/sites-available/domain_one.conf
sudo nano /etc/nginx/sites-available/domain_two.conf
```
Tham khảo file domain_one.conf
```nginx
server {
listen 80;
listen [::]:80;

root /var/www/domain_one.com/html;
index index.html index.htm index.nginx-debian.html;

server_name domain_one.com www.domain_one.com;

location / {
try_files $uri $uri/ =404;
}
}
```
Tham khảo file domain_two.conf
```nginx
server {
listen 80;
listen [::]:80;

root /var/www/domain_two.com/html;
index index.html index.htm index.nginx-debian.html;

server_name domain_two.com www.domain_two.com;

location / {
try_files $uri $uri/ =404;
}
}
```
Copy từ sites-available sang sites-enabled. Chú ý do trong nginx.conf không include sites-available nên phải copy sang *.conf.d hoặc sites-enabled
```sh
sudo ln -s /etc/nginx/sites-available/domain_one.com.conf /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/domain_two.com.conf /etc/nginx/sites-enabled/
```
Hoặc có thể tạo file .conf trực tiếp trong sites-enabled
```sh
sudo nano /etc/nginx/nginx.conf
```
To avoid a possible hash bucket memory problem that can arise from adding additional server names, it is necessary to adjust a single value in the /etc/nginx/nginx.conf file
Tìm đến server_names_hash_bucket_size
Và uncomment (xóa bỏ # đằng trước )
Sau đó check cú pháp file nginx.conf và apply cấu hình
```sh
nginx -t
```
Restart lại nginx
```sh
sudo systemctl restart nginx
```
Cài SSL dùng Cerbot
```sh
sudo apt install certbot python3-certbot-nginx
```
Apply Cerbot vào domain
```sh
sudo certbot --nginx -d example.com -d www.example.com
```
Kết quả
```
Output
Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate number [1-2] then [enter] (press 'c' to cancel):
SSL do Cerbot sinh ra chỉ có thời hạn trong 9 ngày, vì vậy cần phải tự động renew key sau khi hết hạn
sudo systemctl status certbot.timer

Output
● certbot.timer - Run certbot twice daily
Loaded: loaded (/lib/systemd/system/certbot.timer; enabled; vendor preset: enabled)
Active: active (waiting) since Mon 2020-05-04 20:04:36 UTC; 2 weeks 1 days ago
Trigger: Thu 2020-05-21 05:22:32 UTC; 9h left
Triggers: ● certbot.service
```
```sh
sudo certbot renew --dry-run
```

## Chú ý:
Sử dụng nginx ( 172.255.255.50 ) để trỏ đến 1 app trong host (172.255.255.50) có port 30080 ta tham khảo đoạn cấu hình sau
```nginx
upstream mandala-backoffice {
ip_hash;
server 172.255.255.50:30080;
#server localhost:30080;
}

server {
server_name bo.mandalaholiday.com;
location / {
    proxy_pass http://mandala-backoffice;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection keep-alive;
    proxy_set_header X-Forward-Proto $scheme;
    proxy_set_header X-Nginx-Proxy true;
    proxy_cache_bypass $http_upgrade;
    proxy_http_version 1.1;
    #proxy_redirect off;
}
}
```
Nếu nginx(172.155.155.50) trỏ đến app nằm trên host khác (10.0.0.183 ) và port 9000 ta tham khảo cấu hình sau:
```nginx
server {
    server_name cdn.mandalaholiday.com;
    client_max_body_size 1000M;
    ignore_invalid_headers off;
    proxy_buffering off;
    location / {
        proxy_pass http://10.0.0.183:9000;
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Upgrade $http_upgrade;
        # proxy_set_header Connection keep-alive;
        proxy_set_header Connection "";
        proxy_set_header X-Forward-Proto $scheme;
        proxy_set_header X-Nginx-Proxy true;
        proxy_cache_bypass $http_upgrade;
        proxy_http_version 1.1;
        #proxy_redirect off;
        chunked_transfer_encoding off;
    }
}
```
Một domain trỏ đến 2 host khác nhau dựa vào Url
```nginx
upstream mandala-booking {
       ip_hash;
       server 172.255.255.196:30080;
}

server {
        server_name uat.mandalaholiday.com;
        location / {
                proxy_pass              http://mandala-booking;
                proxy_set_header        Host            $host;
                proxy_set_header        X-Real-IP       $remote_addr;
                proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header        Upgrade $http_upgrade;
                proxy_set_header        Connection keep-alive;
                proxy_set_header        X-Forward-Proto $scheme;
                proxy_set_header        X-Nginx-Proxy true;
                proxy_cache_bypass      $http_upgrade;
                proxy_http_version      1.1;
                #proxy_redirect         off;
        }
	location ^~ /evoucher/ {
		        proxy_pass		http://172.255.255.72:3344;
		        proxy_set_header        Host            $host;
                proxy_set_header        X-Real-IP       $remote_addr;
                proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header        Upgrade $http_upgrade;
                proxy_set_header        Connection keep-alive;
                proxy_set_header        X-Forward-Proto $scheme;
                proxy_set_header        X-Nginx-Proxy true;
                proxy_cache_bypass      $http_upgrade;
                proxy_http_version      1.1;
	}
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/uat.mandalaholiday.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/uat.mandalaholiday.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}

server {
        server_name  mandala-admin-uat.mandalaholiday.com;
        location / {
                proxy_pass              http://mandala-booking;
                proxy_set_header        Host            $host;
                proxy_set_header        X-Real-IP       $remote_addr;
                proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header        Upgrade $http_upgrade;
                proxy_set_header        Connection keep-alive;
                proxy_set_header        X-Forward-Proto $scheme;
                proxy_set_header        X-Nginx-Proxy true;
                proxy_cache_bypass      $http_upgrade;
                proxy_http_version      1.1;
                #proxy_redirect         off;
        }

}

Ví dụ cấu hình wordpress
```nginx
server {
	## listen for ipv4; this line is default and implied
        #listen   [::]:80 default_server ipv6only=on; ## listen for ipv6

        root /var/www/mandalahotel.com.vn/public;
        index index.php  index.html index.htm;

        # Make site accessible from http://localhost/
        server_name hotel.mandalaholiday.tk;
	location / {
		try_files $uri $uri/ /index.php$is_args$args;
		root   /var/www/mandalahotel.com.vn/public;
		index index.php index.html index.htm;
       }

	location ^~ /en/ {
                try_files $uri $uri/ /index.php$is_args$args;
                root   /var/www/mandalahotel.com.vn/public/en;
                index index.php index.html index.htm;
       }
	location ~ \.php$ {
		include snippets/fastcgi-php.conf;
		#fastcgi_pass 127.0.0.1:9000;
		fastcgi_pass unix:/run/php-fpm/php7.sock;
		fastcgi_intercept_errors        on;
        		error_page 404 /error/404.php;
	}

	# deny access to .htaccess files, if Apache's document root
	# concurs with nginx's one
	#
	location ~ /\.ht {
		#deny all;
	}
	location ~ /.well-known {  
             allow all;
	}
}
```
Chú ý: Kiểm tra version php-fpm tại /var/run/php-fpm => check file php7.sock hay php?.sock để điền vào nginx

```
Với dự án dùng `PHP` tham khảo
```nginx
server {
    listen 80; 
    listen [::]:80;

    root /var/www/abond.apec.com.vn/public;
    index index.html index.php index.htm index.nginx-debian.html;

    server_name abond.apec.com.vn www.abond.apec.com.vn;
    location / {
        try_files $uri $uri/ /index.php$is_args$args;
        root /var/www/abond.apec.com.vn/public;
        index index.php index.html index.htm;
    }
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.3-fpm.sock;
    }

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    location ~ /\.ht {
        #deny all;
    }
    location ~ /.well-known { 
        allow all;
    }
}
```
### Config trỏ đến domain đến subfolders
Description
Let’s say your website (e.g. mywebsite.com) is installed in the following directory on your server /var/www/mywebsite and you’d like to install a WordPress blog that can be accessed through the following URL mywebsite.com/blog.

You’ll have to configure NGINX to redirect users requests to a particular directory depending on the requested domain subfolder which in your case is mywebsite.com/blog.

There are two ways to do that.

The Subfolder Isn’t Inside Your Main Folder
The directory structure is the following.

```
/var/www/mywebsite
/var/www/blog
```
Open your NGINX config file which should be located in /etc/nginx/sites-available/mywebsite and the following code (change it according to your needs).

```
location /blog {
     alias /var/www/blog;
     try_files $uri $uri/ @blog;
 
     location ~ \.php$ {
             include fastcgi_params;
             fastcgi_param SCRIPT_FILENAME $request_filename;
             fastcgi_pass   unix:/run/php/php7.2-fpm.sock;
     }
}
```
```
location @blog {
     rewrite /blog/(.*)$ /blog/index.php?/$1 last;
}
```
Check for errors and reload the NGING configurations.
```
nginx -t
service nginx reload
```
The Subfolder Is Inside The Main Folder
The directory structure is the following.
```
/var/www/mywebsite
/var/www/mywebsite/blog
```
Open your NGINX config file and the following code (change it according to your needs).
```
  location /blog {
      alias /var/www/mywebsite/blog;
      try_files $uri $uri/ @blog;
 
      location ~ \.php$ {
          include fastcgi_params;
          fastcgi_param SCRIPT_FILENAME $request_filename;
          fastcgi_pass   unix:/run/php/php7.2-fpm.sock;
      }
}
 
location @blog {
      rewrite /blog/(.*)$ /blog/index.php?/$1 last;
}
```
Check for errors and reload the NGING configurations.

```
nginx -t
service nginx reload
```

## Tự động gia hạn ssl
Sử dụng CronbTab để chạy lệnh này cấp mới chứng chỉ. Ví dụ, cứ 2h sáng vào ngày 1 hàng tháng thì chạy, ta tạo crontab như sau:
```
crontab -e
```
Sau đó nhập vào
```
0 2 1 * *  /usr/bin/certbot renew  >/dev/null 2>&1
```
## Tài liệu
[How to Configure Nginx to serve Multiple Websites on a Single VPS](https://webdock.io/en/docs/how-guides/shared-hosting-multiple-websites/how-configure-nginx-to-serve-multiple-websites-single-vps)

[NGINX - Host multiple domains on one server](https://www.learnbestcoding.com/post/15/hosting-multiple-websites-with-nginx)

[How To Install Nginx on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04)

[How To Secure Nginx with Let's Encrypt on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-20-04)
