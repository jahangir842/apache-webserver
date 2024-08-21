## /etc/nginx/sites-enabled$ cat default 
```
# Default server configuration
server {
	listen 80 default_server;
	listen [::]:80 default_server;

	root /var/www/html;
	index index.html index.htm index.nginx-debian.html;

	server_name _;

	location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		try_files $uri $uri/ =404;
	}
}

server {
	root /var/www/tradingmachine;
	index index.php index.html index.htm index.nginx-debian.html;
	server_name app.tradingmachine.ai; # managed by Certbot
	  index index.php index.html index.htm;

    location / {

	    try_files $uri $uri/ /index.html;
    }
    location = /login.php {
        return 301 https://app.tradingmachine.ai/;
    }
    location = /register.php{
	    return 301 https://app.tradingmachine.ai/signup;
    }

 location /api/ {
                proxy_pass  http://127.0.0.1:5577/;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

    		proxy_connect_timeout 9000s;
    		proxy_read_timeout 9000s;
       }

    location ~ \.php$ {
       include fastcgi_params;
      fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;  # Replace with your PHP-FPM socket path
     fastcgi_index index.php;
   fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
   fastcgi_param PATH_INFO $fastcgi_path_info;
	fastcgi_read_timeout 300s;
   }

  location ~ /\.(?!well-known).* {
    deny all;
}
    listen [::]:443 ssl ipv6only=on; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/app.tradingmachine.ai/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/app.tradingmachine.ai/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}
server {
    if ($host = app.tradingmachine.ai) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


	listen 80 ;
	listen [::]:80 ;
    server_name app.tradingmachine.ai;
    return 404; # managed by Certbot


}
server {
	root /var/www/html;
	index index.html index.htm index.nginx-debian.html;
    server_name tradingmachine.5t5.net; # managed by Certbot


	location / {
		try_files $uri $uri/ =404;
	}

    listen [::]:443 ssl; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/tradingmachine.5t5.net/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/tradingmachine.5t5.net/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}
server {
    if ($host = tradingmachine.5t5.net) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


	listen 80 ;
	listen [::]:80 ;
    server_name tradingmachine.5t5.net;
    return 404; # managed by Certbot


}
```
