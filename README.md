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

URL of Hosted Application: [http://www.patternrecognition.io/omenu](http://www.patternrecognition.io/omenu)

## My Detailed Configuration Instructions

Below you will find each and every step I personally took to configure my custom server for this project. **Please note**, as I chose to use Linode for my server and not Vagrant or Amazon Lightsail, my commands may differ from other students.

### Step 1 - Create a new user named grader and grant this user sudo permissions.

1. Log into the server remotely, as the root user, using SSH for security: `$ ssh root@104.237.156.17`. Replace 'root' with your root username, if different.
2. Add a new user, in this instance one called 'grader'. `$ sudo adduser grader`
3. Grant this new user sudo permissions:
   * Create a file in the 'sudoers' (sudo users) directory, with the nano command: `$ sudo nano /etc/sudoers.d/grader`
   * Nano will open with this new file, add the text `grader ALL=(ALL:ALL) ALL`.
   * Save the file, and exit nano.


## Running (For a casual viewer)
- For a live demo of the web application being hosted, [click here](http://www.patternrecognition.io/omenu).


- This live demonstration will be moved in the future, so if it is inactive my apologies! Please [contact me](mailto:admin@patternrecognition.io) if you'd like to see the remotely hosted demo.

## Credits
- [Placeholder](Placeholder)
