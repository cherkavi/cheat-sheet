# apache server

## apache tutorial
* [local](file:///usr/share/doc/apache2/README.Debian.gz)
* [openssl](https://www.digicert.com/easy-csr/openssl.htm)
* [apache with openssl](https://www.digicert.com/kb/csr-ssl-installation/ubuntu-server-with-apache2-openssl.htm)


## apache installation apache settings
[manage httpd](https://httpd.apache.org/docs/current/stopping.html)
```sh
# apache server installation, apache server run, web server run, webserver start
sudo su
yum update -y
yum install -y httpd
service httpd start
chkconfig httpd
chkconfig httpd on
vim /var/www/html/index.html
```

debian apache simple installation
```
#!/bin/sh
sudo apt update
sudo apt install apache2 -y
sudo ufw allow 'Apache'
sudo systemctl start apache2
# Create a new index.html file at  /var/www/html/ path
echo "<html> <head><title>server 01</title> </head> <body><h1>This is server 01 </h1></body> </html>" > /var/www/html/index.html
```

debian apache installation
```sh
# installation
sudo su
apt update -y
apt install -y apache2

# service 
sudo systemctl status apache2.service
sudo systemctl start apache2.service

# change index html
vim /var/www/html/index.html

# Uncomplicated FireWall
ufw app list
ufw allow 'Apache'
ufw status

# enable module
a2enmod rewrite

# disable module
# http://manpages.ubuntu.com/manpages/trusty/man8/a2enmod.8.html
a2dismod rewrite

# enable or disable site/virtual host
# http://manpages.ubuntu.com/manpages/trusty/man8/a2ensite.8.html
a2dissite *.conf
a2ensite my_public_special.conf
```

## apache management
```sh
sudo service apache2 start
sudo service apache2 restart
```

## apache SSL
### activate ssl module
```sh
sudo a2enmod ssl
sudo a2dismod ssl
```

### creating self-signed certificates
```sh
sudo make-ssl-cert generate-default-snakeoil --force-overwrite
```

### check certificates
```sh
sudo ls -la /etc/ssl/certs/ssl-cert-snakeoil.pem
sudo ls -la /etc/ssl/private/ssl-cert-snakeoil.key
```

### cert configuration
```sh
vim /etc/apache2/sites-available/default-ssl.conf
```
```conf
                SSLCertificateFile      /etc/ssl/certs/ssl-cert-snakeoil.pem
                SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key
```

### Generating a RSA private key
```bash
openssl req -new -newkey rsa:2048 \
-nodes -out cherkavideveloper.csr \
-keyout cherkavideveloper.key \
-subj "/C=DE/ST=Bavaria/L=München/O=cherkavi/CN=cherkavi developer" \
```
```sh
vim /etc/apache2/sites-available/default-ssl.conf
```
```conf
SSLCertificateFile "/path/to/www.example.com.cert"
SSLCertificateKeyFile "/path/to/www.example.com.key"
```

### parallel open connection
```
<IfModule mpm_prefork_module>
	  #LoadModule cgi_module modules/mod_cgi.so
    StartServers       5
    MinSpareServers    7
    MaxSpareServers   15
    ServerLimit 600
    MaxRequestWorkers 600
    MaxConnectionsPerChild 0
</IfModule>
```
