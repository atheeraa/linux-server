
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

10.Edit the python files with :
engine = create_engine('postgresql://catalog:password@localhost/catalog')

11. 
sudo apt-get install python-pip
sudo apt-get -qqy install postgresql python-psycopg2
pip install sqlalchemy
sudo pip install Flask
 
 
 
 pip install psycopg2-binary
 sudo python3 database_setup.py
 
 
 sudo nano /etc/apache2/sites-available/veganmarketApp.conf
 
<VirtualHost *:80>
	ServerName 52.24.125.52
	ServerAdmin qiaowei8993@gmail.com
	WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
	<Directory /var/www/FlaskApp/FlaskApp/>
		Order allow,deny
		Allow from all
	</Directory>
	Alias /static /var/www/FlaskApp/FlaskApp/static
	<Directory /var/www/FlaskApp/FlaskApp/static/>
		Order allow,deny
		Allow from all
	</Directory>
	ErrorLog ${APACHE_LOG_DIR}/error.log
	LogLevel warn
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
 
 
 
 sudo service apache2 reload
 
 disable apache 
sudo a2dissite 000-default.conf
 sudo service apache2 reload
