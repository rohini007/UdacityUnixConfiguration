# Udacity - Linux Server Configuration Project

An Udacity Full Stack Web Developer Nanodegree Project.

### Technical Information About the Project

- **Server IP Address:** 3.6.92.127
- **SSH server access port:** 2200
- **SSH login username:** grader
- **Application URL:** http://ec2-3-6-92-127.ap-south-1.compute.amazonaws.com

## Tasks performed
1. Launch your Virtual Machine with your Udacity account
2. Creating the RSA Key Pair
3. Logging In as ubuntu via SSH and Updating the System
    3.1. Logging in as ubuntu via SSH
    3.2. Updating the the System
4. Update and upgrade installed packages
5. Change the SSH port from 22 to 2200
6. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200) and HTTP (port 80)
7. Set up to install updates automatically
8. Configure the local timezone to IST
9. Creating the User grader and Adding it to the sudo Group
    9.1. Creating the User grader
    9.2. Adding grader to the Group sudo
10. Adding SSH Access to the user grader
11. Installing Apache Web Server
12. Installing and Configuring Git
13. Installing and configuring PostgreSQL
14. Setting Up Apache to Run the Flask Application
	13.1 Installing mod_wsgi
	13.2 Git clone and setup your Item Catalog Application project (from your GitHub repository from earlier in the Nanodegree program).
	
## Softwares Installed and configuration changes
* sudo apt-get install apache2
* sudo apt-get install libapache2-mod-wsgi-py3
	* Enable mod_wsgi using: sudo a2enmod wsgi
	* Restart Apache sudo service apache2 restart
* sudo apt-get install postgresql
	* Switch to the postgres user: sudo su - postgres
	* Open PostgreSQL interactive terminal with psql.
	```
  Create the catalog user with a password and give them the ability to create databases:
	postgres=# CREATE ROLE catalog WITH LOGIN PASSWORD 'catalog';
	postgres=# ALTER ROLE catalog CREATEDB;
	Exit psql: \q.
	Switch back to the grader user: exit.
	Create a new Linux user called catalog: sudo adduser catalog. Enter password and fill out information and give sudo privileges to catalog user.
  ```
* sudo apt-get install git
* sudo apt-get install python3-sqlalchemy
* sudo apt-get install python3-flask
* sudo apt-get install python3-flask_sqlalchemy
* sudo apt-get install python3-sqlalchemy
* sudo apt-get install python3-oauth2client
* sudo apt-get install libpq-dev
* sudo apt-get install python3-psycopg2

## Set up and enable a virtual host
1. Create /etc/apache2/sites-available/catalog.conf and add the following lines to configure the virtual host:
```
<VirtualHost *:80>
    ServerName 13.59.39.163
    ServerAlias ec2-13-59-39-163.us-west-2.compute.amazonaws.com
    WSGIScriptAlias / /var/www/catalog/catalog.wsgi
    <Directory /var/www/catalog/catalog/>
    	Order allow,deny
  	    Allow from all
    </Directory>
    Alias /static /var/www/catalog/catalog/static
    <Directory /var/www/catalog/catalog/static/>
  	  Order allow,deny
  	  Allow from all
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    LogLevel warn
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
2. Enable virtual host: sudo a2ensite catalog. The following prompt will be returned:
Enabling site catalog.
To activate the new configuration, you need to run:
  service apache2 reload
3. Reload Apache: sudo service apache2 reload.

## Set up the Flask application
Create /var/www/catalog/catalog.wsgi file add the following lines:

#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/catalog/catalog/")
sys.path.insert(1, "/var/www/catalog/")

from catalog import app as application
application.secret_key = "<Secret key>"
Restart Apache: sudo service apache2 restart

## Disable the default Apache site
1. Disable the default Apache site: sudo a2dissite 000-default.conf. The following prompt will be returned:

Site 000-default disabled.
To activate the new configuration, you need to run:
  service apache2 reload
2. Reload Apache: sudo service apache2 reload

## Folder structure
``` 
/var/www/catalog
    |-- catalog.wsgi
    |__ /catalog             # Our Application Module
        ├── __init__.py
		├── client_secrets.json
		├── database_setup.py
		├── fake_db_populator.py
		├── itemcatalog.db
		├── static
		│   └── style.css
		└── templates
			├── delete_category.html
			├── delete.html
			├── edit_category.html
			├── index.html
			├── items.html
			├── layout.html
			├── login.html
			├── new-category.html
			├── new-item-2.html
			├── new-item.html
			├── update-item.html
			└── view-item.html
```
## Other Helpful Resources

- DigitalOcean [How To Deploy a Flask Application on an Ubuntu VPS](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)
- GitHub Repositories 
	- [adityamehra/udacity-linux-server-configuration](https://github.com/adityamehra/udacity-linux-server-configuration)
	- [kongling893/Linux-Server-Configuration-UDACITY](https://github.com/kongling893/Linux-Server-Configuration-UDACITY)
