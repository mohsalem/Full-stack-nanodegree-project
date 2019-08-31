Running application on desktop
============================================


To run this project, you'll need database software (provided by a Linux virtual machine) and the data to analyze.

The virtual machine
This project makes use of the same Linux-based virtual machine (VM).

install the virtual machine(https://www.virtualbox.org/wiki/Downloads). 
install vagrant(https://www.vagrantup.com/downloads.html).
use VM configuration(https://github.com/udacity/fullstack-nanodegree-vm/blob/master/vagrant/Vagrantfile).


You need to bring the VM up using vagrant up, do so now. Then log into it with vagrant ssh.

A terminal in which we've successfully logged into the virtual machine using "vagrant ssh".
Successfully logged into the virtual machine.

move the data
you need to unzip the project file inside the vagrant folder where you installed the VM



The database includes three tables:

The User table includes user accounts created.
The category table includes the categories' names.
The item table includes the name and description for each item.

after you log into your VM you run commands python database_setup.py and python project2.py

you are now ready to open the website on address: localhost:5000

the website allows you to login with your google account to add items and categories and delete and items you created or you can view available categories and items if you do not want to log in 

Server deployment using Linux Server
============================================


Udacity - Full Stack Web Developer Nanodegree
---------------------------------------------
P3: Linux Server Configuration

This project's objective was to set up a web application server—built from a baseline Linux installation—secured against a number of attack vectors and configured to serve the [Item Catalog Application (Coordinate App)](https://github.com/mohsalem/catalogApp_project2Nanodegree).


Requirements
------------

+ A Web Browser such as [Chrome](https://www.google.com/chrome/browser/) or [Firefox](https://www.mozilla.org/en-US/firefox/new/) is installed.

+ Access to a command line terminal such as [PuTTY](https://www.putty.org/) to remotely connect to the server.


Usage
-----

+ SSH into the Linux server as 'grader' using the provided key.
  ```bash
  $ ssh grader@34.230.229.221 -p 2200 -i ~/.ssh/grader_rsa
  ```
+ Enter passphrase for grader_rsa key (Both the key and the passphrase are included in the "Notes to Reviewer" field).

+ Access the [Item Catalog](http://itemcatalog.com.34.230.229.221.xip.io) being run on the server.


IP Address
----------

+ 34.230.229.221


URL
---

+ http://itemcatalog.com.34.230.229.221.xip.io


Third party used
---
+ Putty
+ AWS firewall

Server Configuration
--------------------

+ Initial server setup
  * Create instance on Amazon Lightsail.
  * Update system packages to most recent versions.
      ```bash
      $ sudo apt-get update
      $ sudo apt-get upgrade
      $ sudo apt-get autoremove
      ```

+ User management
  * Disable remote login as root user.
      ```bash
      $ sudo nano /etc/ssh/sshd_config
      ```
    + Update PermitRootLogin to no and save.
      ```bash
      $ PermitRootLogin no
      ```
    + Restart SSH.
      ```bash
      $ sudo service ssh restart
      ```
  * Create grader user with sudo privileges.
    + Add new user called 'grader' with password.
      ```bash
      $ sudo adduser grader
      ```
    + Grant grader sudo privileges.
      ```bash
      $ sudo nano /etc/sudoers.d/grader
      ```
    + Add the following lines and save.
      ```bash
      # User rules for grader
      grader ALL=(ALL) NOPASSWD:ALL
      ```
  * Setup grader to login with an SSH key.
    + Generate RSA key for grader on physical machine (not remote server).
      ```bash
      $ ssh-keygen
      ```
    + Allow grader to login with password (Update PasswordAuthentication to yes).
      ```bash
      $ sudo nano /etc/ssh/sshd_config
      ```
    + Restart SSH.
      ```bash
      $ sudo service ssh restart
      ```
    + login as grader.
      ```bash
      $ ssh grader@34.230.229.221
      ```
    + Create .ssh directory.
      ```bash
      $ mkdir .ssh
      ```
    + Copy and paste the public RSA key into authorized_keys.
      ```bash
      $ nano .ssh/authorized_keys
      ```
    + Set file permissions.
      ```bash
      $ chmod 700 .ssh
      $ chmod 644 .ssh/authorized_keys
      ```
    + Force key-based authentication (Update PasswordAuthentication to no).
      ```bash
      $ sudo nano /etc/ssh/sshd_config
      ```
    + Restart SSH.
      ```bash
      $ sudo service ssh restart
      ```

+ Firewall Management
  * Update the Amazon Lightsail Firewall to allow SSH through Port 2200.
    + Login to Amazon Lightsail account.
    + Under the 'Instances' tab, click the 3-dot menu icon, and select 'Manage'.
    + Under the 'Networking' tab, go to the 'Firewall' settings.
      * Click '+ Add another' and 'Save' after making the following rule:
        ```bash
        Custom TCP 2200
        ```
  * Enable Port 2200 SSH access on the linux server.
      ```bash
      $ sudo nano /etc/ssh/sshd_config
      ```
    + Add the following line and save.
      ```bash
      $ Port 2200
      ```
    + Restart SSH.
      ```bash
      $ sudo service ssh restart
      ```
  * Disable Port 22 SSH access through the Amazon Lightsail Firewall.
    + Login to Amazon Lightsail account.
    + Under your instance 'Networking' tab, go to the 'Firewall' settings.
      * Click 'Edit rules', then delete SSH rule for port 22, and then save:
        ```bash
        SSH TCP 22
        ```
  * Enable Port 123 NTP access through the Amazon Lightsail Firewall.
    + Login to Amazon Lightsail account.
    + Under your instance 'Networking' tab, go to the 'Firewall' settings.
      * Click '+ Add another' and 'Save' after making the following rules:
        ```bash
        Custom TCP 123
        Custom UDP 123
        ```
  * Setup and enable the Uncomplicated Firewall on the linux server.
      ```bash
      $ sudo ufw default deny incoming
      $ sudo ufw default allow outgoing
      $ sudo ufw allow 2200/tcp
      $ sudo ufw allow www
      $ sudo ufw allow ntp
      $ sudo ufw enable
      ```
  * Reboot the linux server for the Uncomplicated Firewall to update.
    + Login to Amazon Lightsail account.
    + Under the 'Instances' tab, click the 3-dot menu icon, and select 'Manage'.
    + Click the 'Reboot' button.

  * Configure the local timezone to UTC
  - Run `sudo dpkg-reconfigure tzdata` and then choose UTC
 
  * Install Apache
  - `sudo apt-get install apache2`

  * Install mod_wsgi
  - Run `sudo apt-get install libapache2-mod-wsgi python-dev`
  - Enable mod_wsgi with `sudo a2enmod wsgi`
  - Start the web server with `sudo service apache2 start`

  
  * Clone the Catalog app from Github
  - Install git using: `sudo apt-get install git`
  - `cd /var/www`
  - `sudo mkdir catalog`
  - Change owner of the newly created catalog folder `sudo chown -R grader:grader catalog`
  - `cd /catalog`
  - Clone your project from github `git clone https://github.com/mohsalem/catalogApp_project2Nanodegree.git catalog`
  - Create a catalog.wsgi file, then add this inside:
  ```
  import sys
  import logging
  logging.basicConfig(stream=sys.stderr)
  sys.path.insert(0, "/var/www/catalog/")
  
  from catalog import app as application
  application.secret_key = 'supersecretkey'
  ```
  - Rename application.py to __init__.py `mv application.py __init__.py`
  
  * Install virtual environment
  - Install the virtual environment `sudo pip install virtualenv`
  - Create a new virtual environment with `sudo virtualenv venv`
  - Activate the virutal environment `source venv/bin/activate`
  - Change permissions `sudo chmod -R 777 venv`

  * Install Flask and other dependencies
  - Install pip with `sudo apt-get install python-pip`
  - Install Flask `pip install Flask`
  - Install other project dependencies `sudo pip install httplib2 oauth2client sqlalchemy psycopg2 sqlalchemy_utils`

  * Update path of client_secrets.json file
  - `nano __init__.py`
  - Change client_secrets.json path to `/var/www/catalog/catalog/client_secrets.json`
  
  * Configure and enable a new virtual host
  - Run this: `sudo nano /etc/apache2/sites-available/catalog.conf`
  - Paste this code: 
  ```
  <VirtualHost *:80>
      ServerName 34.230.229.221
      ServerAlias HOSTNAME itemcatalog.com.34.230.229.221.xip.io
      ServerAdmin mohamed.amr.salem@gmail.com
      WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/venv/lib/python2.7/site-packages
      WSGIProcessGroup catalog
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
  - Enable the virtual host `sudo a2ensite catalog`

  * Install and configure PostgreSQL
  - `sudo apt-get install libpq-dev python-dev`
  - `sudo apt-get install postgresql postgresql-contrib`
  - `sudo su - postgres`
  - `psql`
  - `CREATE USER catalog WITH PASSWORD 'password';`
  - `ALTER USER catalog CREATEDB;`
  - `CREATE DATABASE catalog WITH OWNER catalog;`
  - `\c catalog`
  - `REVOKE ALL ON SCHEMA public FROM public;`
  - `GRANT ALL ON SCHEMA public TO catalog;`
  - `\q`
  - `exit`
  - Change create engine line in your `__init__.py` and `database_setup.py` to: 
  `engine = create_engine('postgresql://catalog:password@localhost/catalog')`
  - `python /var/www/catalog/catalog/database_setup.py`
  
  * Restart Apache 
  - `sudo service apache2 restart`

  * Update google oauth uri
  - open google console credentials and add the new link to redirect uri


+ The error log file.
  ```bash
  $ sudo less /var/log/apache2/error.log
  ```

+ The access log file.
  ```bash
  $ sudo less /var/log/apache2/access.log
  ```
