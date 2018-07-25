# apache cheatsheet


## how to

[how to check the error logs of apache][apache-error]

[how to see the status of the apache server][status-apache]

[how to set up a reverse proxy][reverseP]

[how to create a load balancer][loadB]

[how to activate/enable a site][enable]

[how to disable a site][disable]

[how to create a virtual host][virtual]

[how to setup a local host file][local]

[apache-error]:#how-to-check-the-error-logs-of-apache
[status-apache]:#how-to-see-the-status-of-the-apache-server
[local]:#how-to-setup-a-local-host-file
[home]:#apache-cheatsheet
[reverseP]:#how-to-set-up-a-reverse-proxy
[loadB]:#how-to-create-a-load-balancer
[enable]:#how-to-enable-a-site
[disable]:#how-to-disable-a-site
[virtual]:#how-to-create-a-virtual-host


### how to check the error logs of apache

<details>
<summary>
View Content
</summary>


```
 sudo vim /var/log/apache2/error.log
```
</details>

[go back :house:][home]


### how to see the status of the apache server

<details>
<summary>
View Content
</summary>

**references**
- [3 Ways to Check Apache Server Status and Uptime in Linux](https://www.tecmint.com/check-apache-httpd-status-and-uptime-in-linux/)

```
sudo systemctl status apache2	
```
</details>

[go back :house:][home]


## how to setup a local host file
this is great for setting creating a websites or at least developing it without
having to buy a domain ... for now . This is essentially working apache for the
most part and then using your personal computer

1. create a conf file for apache
```
sudo cp /etc/apache2/sites-available/000-default.conf  /etc/apache2/sites-available/test.conf
sudo nano /etc/apache2/sites-available/test.conf

// in the test.conf file, add the ServerName test.com and close the file

<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /home/jermaine/test
        ServerName test.com
        <Directory /home/jermaine/test/>
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        <IfModule mod_dir.c>
            DirectoryIndex index.php index.pl index.cgi index.html index.xhtml index.htm
        </IfModule>

</VirtualHost>

```

2. next enable the site

```
sudo a2ensite test.conf

sudo service apache2 reload
```

3. now, go to your personal and run notepad as an administrator

4. once in notepad, open a file to the path `C:\Windows\System32\drivers\etc\hosts`

5. now in the hosts file insert your vps ip address and the ServerName

```
111.111.111.111 test.com
```

6. and that is about it.

[go back home][home]

### how to set up a reverse proxy
**reference**
- [how to use apache as a reverse proxy](https://www.digitalocean.com/community/tutorials/how-to-use-apache-as-a-reverse-proxy-with-mod_proxy-on-ubuntu-16-04)

1. install all of these things
```
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod proxy_balancer
sudo a2enmod lbmethod_byrequests

 sudo service apache2 restart
```
2. update the server
```
sudo apt-get  update
sudo apt-get -y upgrade
```

3. create the conf file like this
```
sudo nano /etc/apache2/sites-available/node.conf

// inside node.conf file

<VirtualHost *:80>
ServerName node.com
ProxyPreserveHost On

ProxyPass / http://127.0.0.1:8080/
ProxyPassReverse / http://127.0.0.1:8080/
</VirtualHost>

```
4. enable the site
```
sudo a2ensite node.conf
sudo service apache2 reload    
```

[go back to home ][home]

### how to create a load balancer

```

```
[go back to home ][home]


### how to enable a site

```
    sudo a2ensite example.conf

    sudo service apache2 reload
```
[go back to home ][home]


### how to disable a site

```
    sudo a2dissite example.conf
```
[go back to home ][home]


### how to create a virtual host

- you can simply copy the default conf file like this
```
    sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/example.conf
    sudo nano example.conf


```
- in the example file make sure you change the document root location and add the
server name

```
    <VirtualHost *:80>
        DocumentRoot "/www/var/html"
        ServerName www.example.com

        # Other directives here
    </VirtualHost>
```
- then add enable the site and reload apache

```
    sudo a2ensite example.conf
    sudo service apache2 reload
```
[go back to home ][home]
