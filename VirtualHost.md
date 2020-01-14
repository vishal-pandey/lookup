### Virtual Host Configuration for wordpress site
```conf
<VirtualHost *:80>  
 ServerName www.example.com  
 ServerAlias www.example.com example.com  
 DocumentRoot /var/www/example/  
  
 <Directory /var/www/example/>  
  AllowOverride All  
  Options Indexes FollowSymLinks MultiViews  
            Order allow,deny  
            allow from all  
            Require all granted  
  
        RewriteBase /  
        RewriteRule ^index\.php$ - [L]  
        RewriteCond %{REQUEST_FILENAME} !-f  
        RewriteCond %{REQUEST_FILENAME} !-d  
        RewriteRule . /index.php [L]  
 </Directory>  
</VirtualHost>
```

### Virtual Host Configuration for reverse proxy
```conf
<VirtualHost *:80>
 ProxyPreserveHost On
 ServerName subdomain.example.com
 ProxyPass "/"  "http://127.0.0.1:5000/"
 ProxyPassReverse "/"  "http://127.0.0.1:5000/"
</VirtualHost>
```
After creating or updating .conf file restart apache server

```sh
sudo service apache2 restart
```
