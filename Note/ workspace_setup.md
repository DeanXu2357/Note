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
* unity-tweak-tool
* gnome-tweak-tool
* update nvidia driver
* arc-theme
* ibus chewing
---
## ibus chewing
```
sudo apt-get install ibus-chewing
ibus restart
```
## update nvidia driver
```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update 
```
## unity-tweak-tool
```
sudo apt-get install unity-tweak-tool
```

## gnome-tweak-tool
```
sudo apt-get install gnome-tweak-tool
```
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

```
sudo apt-get update
sudo apt-get install nginx

sudo ufw allow 'Nginx HTTP'

sudo ufw status
```
---
# mysql

```
sudo apt-get install mysql-server

mysql_secure_installation
```

---
# php

```sudo apt-get install php-fpm php-mysql```

optional 安裝php7.1   
```
sudo apt install software-properties-common (允許使用安裝第三方ppa)
sudo add-apt-repository ppa:ondrej/php (php7.1 ppa)
sudo apt-get update
(optional) sudo apt-get remove php7.0
sudo apt-get install php7.1 php7.1-fpm (from comments)
```

以下為完整板
```
sudo apt install -y nginx php7.1 php7.1-fpm php7.1-cli php7.1-common php7.1-json php7.1-opcache php7.1-mysql php7.1-phpdbg php7.1-mbstring php7.1-gd php7.1-imap php7.1-ldap php7.1-pgsql php7.1-pspell php7.1-recode php7.1-soap php7.1-tidy php7.1-dev php7.1-intl php7.1-curl php7.1-zip php7.1-xml php-xdebug
```

```
cd /etc/php/版本/fpm  
sudo vi php.ini
```
>  將文本內容裡的```;cgi.fix_pathinfo=1```  
改成```cgi.fix_pathinfo=0```
 
為何要改這個？  
基於安全性考量，因為如果設為1(enabled)他匯允許，  
在你call的那隻php找不到時，去尋找相近的php檔案執  
行，想當然爾，這樣會讓我們的網頁執行我們本來不想讓  
他執行的php檔案，從而造成安全性上的問題。

```sudo systemctl restart php7.1-fpm   ```  
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
            try_files $uri $uri/ /index.php?$query_string;;
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
記得結束後執行  
```sudo mv composer.phar /usr/loacl/bin/composer```  
設成全域

## .composer/vendor/bin加到環境變數  
這種方法只有當下那個termainal會加到全域變數
```
export PATH="$PATH:$HOME/.composer/vendor/bin"
```
要執行
```
echo 'export PATH="$PATH:$HOME/.composer/vendor/bin"' >> ~/.bashrc
```
然後
```
source ~/.bashrc
```

> P.S. 上述加到全域變數，最好使用```$HOME```而不是```~```

> 回復環境變數設定值到初始  
> ```/bin/cp /etc/skel/.bashrc ~/```   
> ```source ~/.bashrc``` 
---

# chrome 安裝
官網下載完畢後用```sudo apt install 路徑```安裝下載的.deb檔案即可
---

# Optional 

## xdebug 安裝

到```/etc/php/7.1/mods-available/xdebug.ini```
寫入以下

    zend_extension=xdebug.so # If not already
    xdebug.remote_enable=1
    xdebug.remote_autostart = 1
    xdebug.remote_handler=dbgp
    xdebug.remote_mode=req
    xdebug.remote_host=localhost
    xdebug.remote_port=9000
    xdebug.var_display_max_depth = -1
    xdebug.var_display_max_children = -1
    xdebug.var_display_max_data = -1
    xdebug.idekey = "PHPSTORM" # 如果你是用php storm再複製

## VsCode字體
```"editor.fontFamily": "'Noto Mono', 'Courier New', monospace",```

## Elementary Os Theme

[連結集合](https://www.one-tab.com/page/7UeZ7hnbSWKsLxUXplf3-Q)

## VsCode套件

[來源參考](https://medium.com/@shengyou/2018ironman-eos-for-php-developer-day21-cc0e4b35dc08)

## VsCode 安裝 plant uml

```
sudo apt-get update
sudo apt-get install default-jre
sudo apt-get install default-jdk
sudo apt-get install graphviz
```