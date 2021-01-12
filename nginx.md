# nginx

## managing
```sh
sudo systemctl restart nginx
sudo nginx -s reload
sudo systemctl status nginx
```

[nginx variables](http://nginx.org/en/docs/varindex.html)
```sh
sudo vim /etc/nginx/sites-available/appbridge
cat /etc/nginx/sites-available/appbridge
```

```
server {
        listen 80;
        listen 443;
        listen [::]:80;
        listen [::]:443;

        server_name appbridge.myhost.com;

        location / {
            proxy_set_header    X-Real-IP           $realip_remote_addr;
            proxy_set_header    X-Forwarded-For     $proxy_add_x_forwarded_for;
            proxy_set_header    Host                $http_host;
            proxy_set_header    X-Forwarded-Proto   $scheme;
            proxy_pass          http://localhost:5001/;
            proxy_set_header    X-Forwarded-Host    $host;
            proxy_set_header    X-Forwarded-Server  $server_name;

        }
}
```
