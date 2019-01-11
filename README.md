
The server IP address is :  35.158.243.74

Port is : 2200 


1.To connect to amazon lightsail instance :

Download the key and move it into ~/.ssh and type:

$ ssh -i ~/.ssh/skey.pem -p 2200 ubuntu@35.158.243.74

2. For the grader to access the server the password and the phrase are given in the project submission notes.

$ ssh grader@35.158.243.74 -p 2200 -i ~/lkey


3.Update all currently installed packages

$sudo apt-get update

$sudo apt-get upgrade

4.Configure (UFW)

sudo ufw deny incoming

sudo ufw default allow outgoing 

sudo ufw allow 2200/tcp

sudo ufw allow 80/tcp

sudo ufw allow 123/udp

sudo ufw enable 

5.change the local timezone :

$sudo dpkg-reconfigure tzdata


6. Install the following packages

$sudo apt-get install libapache2-mod-wsgi-py3

$install git

$sudo apt-get install postgresql



7.Don't allow remote access to postgresql

check if remote access is allowed by :

$sudo nano /etc/postgresql/9.5/main/pg_hba.conf


8.Create database user 

Login 

$"sudo su - postgres

$psql

#CREATE USER catalog WITH PASSWORD '*****';

#CREATE DATABASE catalog WITH OWNER catalog;

#\q

exit



9. Create a new directory in /var/www called veganmarket , clone git repository in it

$mkdir /var/www/veganmarket

cd /var/www/veganmarket

$git clone https://github.com/atheeraa/veganmarket.git

10.Create wsgi file in veganmarket dir
$ sudo nano /var/www/veganmarket/veganmarket.wsgi

import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/veganmarket/")

from veganmarket import app as application
application.secret_key = 'super_secret_key'


11.Edit the python files with :

engine = create_engine('postgresql://catalog:password@localhost/catalog')


12. Install nesessary packages:

$sudo apt-get install python-pip

$sudo apt-get -qqy install postgresql python-psycopg2

$pip install sqlalchemy

$sudo pip install Flask

$pip install psycopg2-binary


13. Change database path in __init__.py and database_setup.py and veganmarket.py to 

$engine = create_engine('postgresql://catalog:password@localhost/catalog')

$sudo python3 database_setup.py

$sudo python3 __init__.py

$sudo python3 veganmarket.py

14.Create a virtual environment

$sudo pip install virtualenv

$sudo virtualenv venv

Activate it with

$source venv/bin/activate

Change the permissions

$sudo chmod -R 777 venv
 
15. Create .conf file :

$sudo nano /etc/apache2/sites-available/veganmarketApp.conf
 
<VirtualHost *:80>
        ServerName 35.158.243.74
        ServerAdmin atheerabdullaha@outlook.com
        WSGIScriptAlias / /var/www/veganmarket/veganmarketserver.wsgi
        <Directory /var/www/veganmarket/>
                Order allow,deny
                Allow from all
        </Directory>
        Alias /static /var/www/veganmarket/static
        <Directory /var/www/veganmarket/static/>
                Order allow,deny
                Allow from all
        </Directory>
        ErrorLog ${APACHE_LOG_DIR}/error.log
        LogLevel warn
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

16. Reload server

$sudo disable apache 

$sudo a2dissite 000-default.conf

$sudo service apache2 reload

17. Configure google Api console and add new domain and redirect urls
18. Visit http://35.158.243.74.xip.io
