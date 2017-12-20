# 工作環境架設筆記
## 前言
在公司重新架設elementary.os環境的紀錄，主要安裝linux上的php開發工具
---
## 目錄
* vs code
    * xdebug
    * phpcs
    * php-cs-fixer
* git
* chrome
* LEMP
    * linux
    * nginx
    * mysql
    * php
* composer
---
## Git

```
$ sudo apt-get install git
```
---
## Vs Code
1. 官網下載.deb檔案
2. 執行```$ sudo apt install 絕對路徑```以安裝檔案
---
# Nginx

sudo apt-get update
sudo apt-get install nginx

sudo ufw allow 'Nginx HTTP'

sudo ufw status
---
# mysql

sudo apt-get install mysql-server

mysql_secure_installation

---
# php

sudo apt-get install php-fpm php-mysql

    optional 安裝php7.1   
    sudo apt install software-properties-common
    sudo add-apt-repository ppa:ondrej/php
    sudo apt-get update
    (optional) sudo apt-get remove php7.0
    sudo apt-get install php7.1 php7.1-fpm (from comments)


cd /etc/php/版本/fpm  
sudo vi php.ini
>  將文本內容裡的```;cgi.fix_pathinfo=1```  
改成```cgi.fix_pathinfo=0```
 
為何要改這個？  
基於安全性考量，因為如果設為1(enabled)他匯允許，  
在你call的那隻php找不到時，去尋找相近的php檔案執  
行，想當然爾，這樣會讓我們的網頁執行我們本來不想讓  
他執行的php檔案，從而造成安全性上的問題。

sudo systemctl restart php7.1-fpm   
重新啟動php，php fpm 的地方具體看你是用哪個版本去呼叫

修改nginx設定檔  
位置：/etc/nginx/site-available
sudo vi default

原本的檔案內容  

    server {
        listen 80 default_server;
        listen [::]:80 default_server;

        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
            try_files $uri $uri/ =404;
        }
    }

我們修改成

    server {
        listen 80 default_server;
        listen [::]:80 default_server;
    
        root /var/www/html;
        index index.php index.html index.htm index.nginx-debian.html;
    
        server_name server_domain_or_IP;
    
        location / {
            try_files $uri $uri/ =404;
        }
    
        location \.php$ {
            include snippets/fastcgi-php.conf;
            fastcgi_pass unix:/run/php/php7.0-fpm.sock;
        }
    
        location ~ /\.ht {
            deny all;
        }
    }
---
# composer 安裝

參照官網  
記得結束後執行```sudo mv composer.phar /usr/loacl/bin/composer```設成全域

---

# chrome 安裝
官網下載完畢後用```sudo apt install 路徑```安裝下載的.deb檔案即可