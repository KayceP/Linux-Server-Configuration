![Part of the Udacity Full Stack Web Developer Nanodegree](https://img.shields.io/badge/Udacity-Full%20Stack%20Web%20Developer%20Nanodegree-blue.svg)
> Developed for project 6 of 6 as part of the **Udacity Full Stack Web Developer Nanodegree**.

# Linux Server Configuration
My personal set of server configuration instructions for the  [Udacity Full Stack Web Developer Nanodegree](https://www.udacity.com/uconnect/intensive/full-stack-web-developer-nanodegree) Linux Server Project, hosted on a Linode server, running Ubuntu 16.04 LTS.

<p align="center">
<img src="https://i.imgur.com/cx0bJYF.png">
</p>

## Project Overview

For this project, we were tasked with taking a baseline installation of a Linux distribution and preparing it to host a previous web application we built, the [Item Catalog Project](https://github.com/KayceP/OMenu).

This included installing updates, additional packages, securing it from a number of attack vectors and installing/configuring web and database servers.

Our reviewer was issued a login with root access, and had to log into our server and validate our work, and tested the server/web application for functonality and security.

The full steps I took to complete the project are documented below.

This project utilizes:

- Linode (A VPS (Virtual Private Server)) Provider
- Ubuntu (16.04 LTS), using:
  - Uncomplicated Firewall (UFW)
  - Finger
  - Apache
  - Mod_wsgi
  - Git
  - Pip
  - Virtualenv
  - Flask
  - Bleach
  - Httplib2
  - Libpq-dev
  - Python-dev
  - Request
  - Oauth2client
  - Sqlalchemy
  - Psycopg2
  - PostgreSQL
  - Postgresql-contrib
  - Glances

## User Requirements (If setting this up yourself)

- Access to a server running Linux, perferably Ubuntu 16.04 LTS.
- Shell access to the server, including root.

## Server Information (For grader)

IP Address: 104.237.156.17

Open SSH Port: 2200

Private Key: Only shared with our grader at Udacity.

Sudo Password once logged in via SSH: Only shared with our grader at Udacity.

URL of Hosted Application: [http://www.patternrecognition.io](http://www.patternrecognition.io)

## My Detailed Configuration Instructions

Below you will find each and every step I personally took to configure my custom server for this project. **Please note**, as I chose to use Linode for my server and not Vagrant or Amazon Lightsail, my commands may differ from other students.

### Step 1 - Create a new user named grader and grant this user sudo permissions.

1. Log into the server remotely, as the root user, using SSH for security: `$ ssh root@104.237.156.17`. 
   * Replace 'root' with your root username, if different.
2. Add a new user, in this instance one called 'grader'. `$ sudo adduser grader`
3. Grant this new user sudo permissions:
   * Create a file in the 'sudoers' (sudo users) directory, with the nano command: `$ sudo nano /etc/sudoers.d/grader`
   * Nano will open with this new file, add the text `grader ALL=(ALL:ALL) ALL`.
   * Save the file, and exit nano.
   
### 2 - Update and and all currently installed packages on the server.

1. Run the command `$ sudo apt-get update`. _(This may take some time)_
2. Once finished, run the command `$ sudo apt-get upgrade`.
   * Enter `Y` when prompted to continue the operation.
3. For Linode servers only, you'll recieve a warning about 'grub-pc' when running these commands:

<p align="center">
<img src="https://i.imgur.com/oH9AUmj.png">
</p>

4. Select 'Keep the local version currently installed', so you can continue to use many features of Linode's server manager.

### 3 - Install [finger](https://linux.die.net/man/1/finger), a utility software to check users' status.
1. Run the command `$ apt-get install finger`.

### 4 - Configure SSH key-based authentication for our 'grader' user.
1. First, create the SSH encryption key on your local machine, with the command: `ssh-keygen -f /home/.ssh/udacity_grader_ssh_key.rsa`. _(You can change the filename, of course)_
   * I used my own personal Raspberry Pi Linux server for this.
2. Log into your remote server as your root user, via ssh.
3. Run the command: `$ touch /home/grader/.ssh/authorized_keys`.
   * Copy the content of the udacity_grader_ssh_key.pub file from your local machine to the /home/grader/.ssh/authorized_keys file you just created on the remote VM.
4. Update permissions for .ssh, and authorized_keys:
   * `$ sudo chmod 700 /home/grader/.ssh`.
   * `$ sudo chmod 644 /home/grader/.ssh/authorized_keys`.
5. Finally change the owner from root to grader: `$ sudo chown -R grader:grader /home/grader/.ssh`.
6. Now you are able to log into the remote VM through ssh with the following command: `$ ssh -i ~/.ssh/udacity_grader_ssh_key.rsa grader@104.237.156.17`.

### 5 - Force key-based authentication as the only login method.
1. Run the command `$ sudo nano /etc/ssh/sshd_config`. 
2. Within this file, locate the field 'PasswordAuthentication' and change the variable from 'yes' to 'no'.
3. Refresh the ssh service by running `$ sudo service ssh restart`.

### 6 - Change the SSH port from 22 to 2200
1. Run the command `$ sudo nano /etc/ssh/sshd_config`. 
2. Within this file, locate the first 'Port' line near the top, and change the variable to '2200'.
3. Refresh the ssh service by running `$ sudo service ssh restart`.
4. Now you _(and the grader)_ are able to log into the server via ssh with the following command: `$ ssh -i udacity_grader_ssh_key.rsa -p 2200 grader@104.237.156.17`.

### 7 - Disable default login for root
1. Run the command `$ sudo nano /etc/ssh/sshd_config`. 
2. Within this file, locate the line 'PermitRootLogin' and change the variable from 'yes' to 'no'.
3. Refresh the ssh service by running `$ sudo service ssh restart`.

### 8 - Configure Uncomplicated Firewall (UFW)

1. Our project requires that our server only allows incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).
2. To do this, execute the following commands once at a time:
   * `$ sudo ufw allow 2200/tcp`.
   * `$ sudo ufw allow 80/tcp`.
   * `$ sudo ufw allow 123/udp`.
   * `$ sudo ufw enable`.
   
### 9 - Install Apache, and mod_wsgi
1. Install Apache by running the command `$ sudo apt-get install apache2`.
2. Once installed, we'll then want to install 'mod_wsgi' as well. This is an Apache addon that allows Apache to serve Flask applications. 
3. Install mod_wsgi by running the command `$ sudo apt-get install libapache2-mod-wsgi python-dev`.
4. Then ensure the newly installed mod_wsgi is running, with `$ sudo a2enmod wsgi`.
5. Start the web server. `$ sudo service apache2 start`.

### 10 - Install and Configure Git
1. Install Git by running the command `$ sudo apt-get install git`.
2. Configure your username: `$ git config --global user.name <username>`.
3. Configure your email: `$ git config --global user.email <email>`.

### 11 - Clone your Catalog app from Github
1. Let's make a new folder in our Apache 'www', for the catalog. 
   * Go to: `$ cd /var/www`.
   * Make the directory: `$ sudo mkdir catalog`.
2. Change owner for the catalog folder: `$ sudo chown -R grader:grader catalog`.
3. Go inside your new folder: `$ cd catalog`
4. Clone the catalog repository from Github: `$ git clone https://github.com/iliketomatoes/catalog.git catalog`.
5. Make a file named `catalog.wsgi` to serve the application over the mod_wsgi. That file should look like this:

```python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/catalog/")

from catalog import app as application
```

### 12 - Install virtual environment, Flask and the project's dependencies
1. Install *pip*, the tool for installing Python packages: `$ sudo apt-get install python-pip`.
2. If *virtualenv* is not installed, use *pip* to install it using the following command: `$ sudo pip install virtualenv`.
3. Move to the *catalog* folder: `$ cd /var/www/catalog`. Then create a new virtual environment with the following command: `$ sudo virtualenv venv`.
4. Activate the virtual environment: `$ source venv/bin/activate`.
5. Change permissions to the virtual environment folder: `$ sudo chmod -R 777 venv`.

### 13 - Install Flask and the project's dependencies
1. Install Flask: `$ pip install Flask`.
2. Install all the other project's dependencies: `$ pip install bleach httplib2 request oauth2client sqlalchemy psycopg2`. 

Sources for steps #12 and #13: [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps), [Dabapps](http://www.dabapps.com/blog/introduction-to-pip-and-virtualenv-python/).

### 14 - Configure and enable a new virtual host

1. Create a virtual host conifg file with the command: `$ sudo nano /etc/apache2/sites-available/catalog.conf`.
2. Paste in the following lines of code:
```
<VirtualHost *:80>
    ServerName 104.237.156.17
    ServerAlias 
    ServerAdmin admin@104.237.156.17
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
* The **WSGIDaemonProcess** line specifies what Python to use and can save you from a big headache. In this case we are explicitly saying to use the virtual environment and its packages to run the application.

3. Enable the new virtual host: `$ sudo a2ensite catalog`.

Source: [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-run-django-with-mod_wsgi-and-apache-with-a-virtualenv-python-environment-on-a-debian-vps).

### 15 - Install and configure PostgreSQL

1. Install some necessary Python packages for working with PostgreSQL: `$ sudo apt-get install libpq-dev python-dev`.
2. Install PostgreSQL: `$ sudo apt-get install postgresql postgresql-contrib`.
3. Postgres is automatically creating a new user during its installation, whose name is 'postgres'. That is a tusted user who can access the database software. So let's change the user with: `$ sudo su - postgres`, then connect to the database system with `$ psql`.
4. Create a new user called 'catalog' with his password: `# CREATE USER catalog WITH PASSWORD 'postgres';`.
5. Give *catalog* user the CREATEDB capability: `# ALTER USER catalog CREATEDB;`.
6. Create the 'catalog' database owned by *catalog* user: `# CREATE DATABASE catalog WITH OWNER catalog;`.
7. Connect to the database: `# \c catalog`.
8. Revoke all rights: `# REVOKE ALL ON SCHEMA public FROM public;`.
9. Lock down the permissions to only let *catalog* role create tables: `# GRANT ALL ON SCHEMA public TO catalog;`.
10. Log out from PostgreSQL: `# \q`. Then return to the *grader* user: `$ exit`.
11. Inside the Flask application, the database connection is now performed with: 
```python
engine = create_engine('postgresql://catalog:postgres@localhost/catalog')
```
12. Setup the database with: `$ python /var/www/catalog/catalog/setup_database.py`.
13. To prevent potential attacks from the outer world we double check that no remote connections to the database are allowed. Open the following file: `$ sudo nano /etc/postgresql/9.5/main/pg_hba.conf` and edit it, if necessary, to make it look like this: 
```
local   all             postgres                                peer
local   all             all                                     peer
host    all             all             127.0.0.1/32            md5
host    all             all             ::1/128                 md5
```
Source: [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps).

### 16 - Install system monitor tools

1. `$ sudo apt-get update`.
2. `$ sudo apt-get install glances`.
3. To start this system monitor program just type this from the command line: `$ glances`.
4. Type `$ glances -h` to know more about this program's options.

Source: [eHowStuff](http://www.ehowstuff.com/how-to-install-and-use-glances-system-monitor-in-ubuntu/).

### 17 - Restart Apache to launch the app

1. `$ sudo service apache2 restart`.

### 18 - Install system monitor tools.

1. `$ sudo apt-get update`.
2. `$ sudo apt-get install glances`.
3. To start this system monitor program just type this from the command line: `$ glances`.
4. Type `$ glances -h` to know more about this program's options.

## Running (For a casual viewer)
- For a live demo of the web application being hosted, [click here](http://www.patternrecognition.io/omenu).
- This live demonstration will be moved in the future, so if it is inactive my apologies! Please [contact me](mailto:admin@patternrecognition.io) if you'd like to see the remotely hosted demo.

## Credits
- [iliketomatoes](https://github.com/iliketomatoes/linux_server_configuration)
- [RobD30](https://github.com/robd30/) (All his encouragement/advice/tips and tricks)
