# FuelPHP + Nginx
## 概要
Nginx 上で動かせるようにした。(最低限の設定)

## .conf
```nginx
    server {
        listen       80;
        charset      utf-8;

        root         /var/www/html/fuelx/public;
        server_name  fuelx.localhost;

        index        index.php index.html index.htm;
        include      /etc/nginx/drop;

        location / {
        	try_files $uri $uri/ /index.php?$args;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   /usr/share/nginx/html;
        }

        location ~ [^/]\.php(/|$) {
                fastcgi_split_path_info ^(.+?\.php)(/.*)$;
                if (!-f $document_root$fastcgi_script_name) {
                        return 404;
                }
                fastcgi_pass 127.0.0.1:9000;
                fastcgi_index  index.php;
                fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
                fastcgi_param  PATH_INFO $fastcgi_path_info;
                fastcgi_param  REQUEST_FILENAME  $request_filename;
                include fastcgi_params;
        }
    }
```
