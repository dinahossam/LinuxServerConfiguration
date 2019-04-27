# LinuxServerConfiguration

 IP address: 13.233.236.126 </br>
 Port: 2200 </br>
 Web Application URL : http://13.233.236.126.xip.io</br>
 
 # Software installed and configuration changes made
  1- Creat an account on  Amazon Web Services, and login to Lightsail.</br>
  2- Create Ubuntu instance and SSH into the Created Server.</br>
  3- Update all currently installed packages.
  ```bash
  $ sudo apt-get update
  $ sudo apt-get udgrade
  ```
  4- Change the SSH port From 22 to 2200 by Changing the port in sshd_config File from 22 to 2200, and also Configure lightsail to allow it.
  ```bash
  $ sudo nano /etc/ssh/sshd_config
   ```
  5- Configure UFW  to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).
  ```bash
  $ sudo ufw allow 2200
  $ sudo ufw allow www
  $ sudo ufw allow ntp
   ```
   and to turn on the firewall.
   ```bash
   $ sudo ufw 
   ```
  6- Create a new user account named grader.
  ```bash
  $ sudo adduser grader
  ```
  7- Give grader the permission to sudo, edit the sudoers file in order to grant grader access to the sudo command
  ```bash
  $ sudo nano /etc/sudoers.d/grader
  ```
  then add this line
  ```
  "grader ALL=(ALL:ALL) ALL"
  ```
  8- Create an SSH key pair for grader using the ssh-keygen tool. </br>
    a- in your local machine, Use SSH-Kegyen to create Private key, 2 files will be created "grader.pub" and "grader".</br>
    b- in the Server Using User Grader.
     ```bash
     $ sudo mkdir .ssh
     $ sudo nano authorized_keys
     ```
     Then copy the content of "grader.pub" into authorized_keys.</br>
  9- Change the Persmissions of .ssh dir and authorized_keys.
  ```bash
  $sudo chmod 700 .ssh
  $sudo chmod 400 /.ssh/authorized_keys
  ```
  10- Configure the local timezone to UTC.
  ```bash
  $ sudo dpkg-reconfigure tzdata
  ```
  11- Install and configure Apache to serve a Python mod_wsgi application.
  ```bash
  $ sudo apt-get install apache2.
  $ sudo apt-get install libapache2-mod-wsgi
  $ sudo apt-get install python-setuptools
  ```
  12- Install and configure PostgreSQL.
  ```bash
  $ sudo apt-get install postgresql
  ```
  13- Do not allow remote connections, make sure that this File '/etc/postgresql/9.5/main/pg_hba.conf' doesn't allow remote connections.</br>
  14- Create a new database user named catalog that has limited permissions to your catalog application database.
  ```bash
  $ sudo su - postgres
  postgres $ psql
  postgres=# CREATE DATABASE catalog;
  # CREATE USER catalog;
  # ALTER ROLE catalog WITH PASSWORD 'password'
  # GRANT ALL PRIVILEGES ON DATABASE catalog TO catalog;
  ```
  15- Install git.
  ```bash
  $ sudo apt-get install git
  ```
  16- Download the ItemCatalog in the path /var/www/ItemCatalog/ using git.</br>
  17- Create wsgi.py file in the path /var/www/ItemCatalog.
  ```bash
  $ sudo nano /var/www/ItemCatalog/wsgi.py
  ```
  and add these Lines.
  import sys
  sys.path.insert(0, '/var/www/ItemCatalog/')
  from ItemCatalog import app as application
  application.config['SQLALCHEMY_DATABASE_URI'] = (
    'postgresql://'
    'catalog:password@localhost/catalog')
  application.secret_key = 'super_secret_key'
  18- update apache2 sites-available to add ItemCatalog site.
  ```bash
  $ sudo nano /etc/apache2/sites-available/ItemCatalog.conf
  ```
  Then add these Lines.
   ```
  <VirtualHost *:80>
    ServerName 13.233.236.126
    ServerAlias 13.233.236.126.xip.io
    WSGIScriptAlias / /var/www/ItemCatalog/wsgi.py
    <Directory /var/www/ItemCatalog>
        Order allow,deny
        Allow from all
    </Directory>
  </VirtualHost>
  ```
  19- Inorder to start Apache server.
  ```bash
  $ sudo a2ensite ItemCatalog
  $ sudo service apache2 reload
  ```
  20- Now Visit the URL http://13.233.236.126.xip.io and it will work.
  
# Reference
  1- https://lightsail.aws.amazon.com </br>
  2- http://xip.io/


  
  
  
  
  
     
     
    
     
    
