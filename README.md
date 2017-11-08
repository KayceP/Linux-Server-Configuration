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
  - Fail2ban
  - Sendmail
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

URL of Hosted Application: [http://www.patternrecognition.io/omenu](http://www.patternrecognition.io/omenu)

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

## Running (For a casual viewer)
- For a live demo of the web application being hosted, [click here](http://www.patternrecognition.io/omenu).
- This live demonstration will be moved in the future, so if it is inactive my apologies! Please [contact me](mailto:admin@patternrecognition.io) if you'd like to see the remotely hosted demo.

## Credits
- [iliketomatoes](https://github.com/iliketomatoes/linux_server_configuration)
- [RobD30](https://github.com/robd30/) (All his encouragement/advice/tips and tricks)
