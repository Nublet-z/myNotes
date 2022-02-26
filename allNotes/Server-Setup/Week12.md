# Server Setup Week 12 (12/20/2021)

## Make User Login Before Accessing Webpage
First we need to start httpd and disable firewall to let our website could be accessed :
```
sudo systemctl start httpd
sudo systemctl stop firewalld
```
after that go to `/var/www/html` directory and make `files` directory. Change directory into `files` and add file into it, for example we add a.htm file that contain string "aaa" `echo "aaa" > a.htm`. After that edit `/etc/httpd/conf/httpd.conf` file and add this several line at the bottom of the file :
```
<Directory /var/www/html/files>
  AllowOverride AuthConfig
</Directory>
```

## Set Up Virtual Host to Enable Multiple Website in One Device
To set up virtual host, you need to go to `/etc/httpd/conf.d` directory and edit `vhosts.conf`. In this directory, write the following text :
```
<VirtualHost _default_:80>
   ServerName www.example.com
   DocumentRoot /var/www/html
</VirtualHost>
<VirtualHost *:80>
   ServerName test1.example.com
   DocumentRoot /var/www/vhosts/test1
</VirtualHost>
<VirtualHost *:80>
   ServerName test2.example.com
   DocumentRoot /var/www/vhosts/test2
</VirtualHost>
```
From the line above, you create the default website as www.example.com with its directory is `/var/www/html` while the other website is a virtual website. Next create `/var/www/vhosts/test1` and `/var/www/vhosts/test2` directory, add `index.html` and write content to differentiate between test1 and test2. For example I do the below command to differentiate them :
```
echo "test1.example.com" > /var/www/vhosts/test1/index.html
echo "test2.example.com" > /var/www/vhosts/test2/index.html
```
After that restart the httpd by typing `sudo systemctl restart httpd`. In windows, open the host file in `C:\Windows\System32\drivers\etc`, edit hosts file in admin mode and add this several line at the bottom :
```
192.168.56.107 www.example.com
192.168.56.107 test1.example.com
192.168.56.107 test2.example.com
```

and you'll able to access those three websites from your windows