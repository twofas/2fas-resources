# How to enable WebDav Sync With Your Server in 2FAS PASS
# Video link: https://youtu.be/jSZiHqH2O_k

```bash
- sudo apt update
- sudo apt upgrade
- sudo apt install apache2
- sudo a2enmod dav
- sudo a2enmod dav_fs
- sudo mkdir -p /var/www/webdav
- sudo chown www-data:www-data /var/www/webdav
- sudo chmod 770 /var/www/webdav
- sudo htpasswd -c /etc/apache2/webdav.password 2fas
- sudo nano my-secure-vault.com.conf
 ```

# my-secure-vault.com.conf content:
```bash
<VirtualHost *:443>
    ServerName my-secure-vault.com
 
    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/my-secure-vault.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/my-secure-vault.com/privkey.pemy
 
    DocumentRoot /var/www/html
    Alias /webdav /var/www/webdav
 
    <Directory /var/www/webdav>
        DAV On
        AuthType Basic
        AuthName "WebDAV Restricted"
        AuthUserFile /etc/apache2/webdav.password
        Require valid-user
 
        Options Indexes
    </Directory>
 
    ErrorLog ${APACHE_LOG_DIR}/webdav_error.log
    CustomLog ${APACHE_LOG_DIR}/webdav_access.log combined
</VirtualHost>
```
```bash
- sudo a2enmod ssl
- sudo a2ensite my-secure-vault.com.conf
- sudo systemctl reload apache2
- sudo apt install certbot python3-certbot-apache -y
- sudo certbot —apache -d my-secure-vault.com
