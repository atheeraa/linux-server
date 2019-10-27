## This is udacity's FSND project, deploying a website on a linux server.

The server IP address is :  **35.158.243.74**

Port is : **2200** 
 
URL : **http://35.158.243.74.xip.io/**



**1. To connect to amazon lightsail instance :**

Download the key and move it into ~/.ssh and type:

>$ ssh -i ~/.ssh/skey.pem -p 22 ubuntu@35.158.243.74


after changing the port into 22 by : 
 
>$sudo nano /etc/ssh/sshd_config

Change the port from 22 to 2200

>$sudo service ssh restart

we will be logging in using 

>$ssh -i ~/.ssh/skey.pem -p 2200 ubuntu@35.158.243.74

**2.Created the grader user and gave it sudo privilges **

>$sudo adduser grader 

>$sudo visudo :

edit it and the line 

>grader ALL=(ALL:ALL) ALL

For the grader to access the server the private key and the phrase are given in the project submission notes.

>$ ssh grader@35.158.243.74 -p 2200 -i ~/grader

create a key pair locally

>$ssh-keygen

copy the .pub file contents into 

>grader : ~/.ssh/authorized_keys

change the permission:

>chmod 700 .ssh

>chmod 644 .ssh/authorized_keys


**3.Update all currently installed packages**

>$sudo apt-get update

>$sudo apt-get upgrade

**4.Configure (UFW)**

>sudo ufw deny incoming

>sudo ufw default allow outgoing 

>sudo ufw allow 2200/tcp

>sudo ufw allow 80/tcp

>sudo ufw allow 123/udp

>sudo ufw enable 


**5.change the local timezone :**

>$sudo dpkg-reconfigure tzdata



**6. Install the following packages**

>$sudo apt-get install libapache2-mod-wsgi-py3

>$install git

>$sudo apt-get install postgresql




**7.Don't allow remote access to postgresql**

check if remote access is allowed by :

>$sudo nano /etc/postgresql/9.5/main/pg_hba.conf



**8.Create database user **

Login 

>$"sudo su - postgres

>$psql

>#CREATE USER catalog WITH PASSWORD '*****';

>#CREATE DATABASE catalog WITH OWNER catalog;

>#\q

>exit



**9. Create a new directory in /var/www called veganmarket , clone git repository in it**

>$mkdir /var/www/veganmarket

>cd /var/www/veganmarket

>$git clone https://github.com/atheeraa/veganmarket.git


**10.Create wsgi file in veganmarket directory**

>$sudo nano /var/www/veganmarket/veganmarket.wsgi

>import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/veganmarket/")

from veganmarket import app as application
application.secret_key = 'super_secret_key'





**11. Install nesessary packages:**

>$sudo apt-get install python-pip

>$sudo apt-get -qqy install postgresql python-psycopg2

>$pip install sqlalchemy

>$sudo pip install Flask

>$pip install psycopg2-binary



**12. Change database path in __init__.py and database_setup.py and veganmarket.py to ** engine = create_engine('postgresql://catalog:password@localhost/catalog')

>$sudo python3 database_setup.py

>$sudo python3 __init__.py

>$sudo python3 veganmarket.py


**13.Create a virtual environment**

>$sudo pip install virtualenv

>$sudo virtualenv venv

Activate it with

>$source venv/bin/activate

Change the permissions

>$sudo chmod -R 777 venv
 

**14. Create .conf file :**

>$sudo nano /etc/apache2/sites-available/veganmarket.conf
><VirtualHost *:80>
    ServerName 35.158.243.74
    ServerAlias
    ServerAdmin admin@35.158.243.74
    WSGIProcessGroup veganmarket
    WSGIDaemonProcess veganmarket python-path=/var/www/veganmarket:/var/www/veganmarket/venv/lib/python2.7/site-packages
    WSGIScriptAlias / /var/www/veganmarket.wsgi
    <Directory /var/www/veganmarket/veganmarket/>
        Order allow,deny
        Allow from all
    </Directory>
    Alias /static /var/www/veganmarket/veganmarket/static
    <Directory /var/www/veganmarket/veganmarket/static/>
        Order allow,deny
        Allow from all
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    LogLevel warn
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>



**15. Reload server**

>$sudo disable apache 

>$sudo a2dissite 000-default.conf

>$sudo service apache2 reload


**16. Configured google Api console and added new domain and redirect urls**

**17. Visit http://35.158.243.74.xip.io**



Resources:
http://flask.pocoo.org/docs/1.0/deploying/mod_wsgi/
https://github.com/FahadAlsubaie/linux_server_configuration
https://github.com/kongling893/Linux-Server-Configuration-UDACITY
https://serverfault.com/questions/294101/wsgidaemonprocess-specifying-a-user
https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps
https://stevenwooding.com/linux-server/
https://lightsail.aws.amazon.com/ls/docs/en/articles/lightsail-how-to-set-up-ssh
