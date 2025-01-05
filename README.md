# Frappe-Installation
Frappe Web Framework Installation, A list of various frappe installation methods documented for future reference.

## Version 15 (Ubuntu 22.04 LTS)
<h3><b> Step 1: Install WSL and Ubuntu </b></h3>

In windows powershell (Run as Administrator) run 

    wsl --install -d ubuntu

If you face an error while opening the installed distro in wsl (Even after restarting the system), try

    wsl --update


After creating a new username and password, run the following in the Linux terminal
    
    sudo apt-get update

Followed by

    sudo apt-get upgrade
      
<h3><b> Step 2: Install Frappe Pre-requisites </b></h3>

We need to install the following prerequisite packages for Frappe V15

    Python 3.11    pip 20+  
    Node.js 18             yarn 1.12+ 
    MariaDB 10.6.6+        cron
    Redis 6                NGINX
    wkhtmltopdf (version 0.12.6 with patched qt)

#### Python 3.11 & pip 20+
Use the following command to install the latest version of python 3
    
    sudo apt-get install python3.11

Then run the following to install the latest version of pip

    sudo apt install python3-pip

We also need the venv package to install frappe

    sudo apt-get install virtualenv
    sudo apt install python3.11-venv
    
#### Node.js 18
To install node.js we first install nvm

    curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash

To use nvm immediately, run
    
    source ~/.profile

Then install the correct version of node using

    nvm install 18.12.0

#### Yarn 1.12+
To install yarn we need to install npm first

    sudo apt-get install npm

Then we install the latest version of yarn using

    npm install -g yarn

#### Redis 6
Run the following command to install the latest version of redis

    sudo apt-get install redis-server

#### wkhtmltopdf 12.6 (with patched qt)
Run the following set of commands to install the correct version of wkhtmltopdf and its dependencies

    sudo apt-get install xfonts-75dpi
    sudo apt-get install xfonts-base
    wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-2/wkhtmltox_0.12.6.1-2.jammy_amd64.deb
    sudo dpkg -i wkhtmltox_0.12.6.1-2.jammy_amd64.deb
    rm wkhtmltox_0.12.6.1-2.jammy_amd64.deb

#### mariadb 10.6.6+
Run the following commands to install mariadb

    sudo apt install software-properties-common
    sudo apt-get update
    sudo apt-get install mariadb-server
    sudo mysql_secure_installation

(During the initial input for root password, just press enter and when prompted to change the root password, change it.)

Now we need to make the following changes to the mariadb configurations

    sudo nano /etc/mysql/my.cnf

Add the following to the end of the file and save

    [mysqld]
    character-set-client-handshake = FALSE
    character-set-server = utf8mb4
    collation-server = utf8mb4_unicode_ci

    [mysql]
    default-character-set = utf8mb4

Then restart the server

    sudo service mariadb restart

Or stop and start the server if you face any issues with restart

    sudo service mariadb stop
    sudo service mariadb start
    
<h3><b> Step 3: Install Frappe Bench </b></h3>
Now that we've installed all the required prerequisites, we can install frappe.

#### Frappe Bench
Install frappe bench using the following pip command

    sudo pip3 install frappe-bench

(If you're encountering is related to PEP 668, which marks certain Python environments as "externally managed" to prevent breaking system-level Python installations)
    
    Create a virtual environment: python3 -m venv ~/frappe-bench-env
    Activate the virtual environment: source ~/frappe-bench-env/bin/activate
    Install frappe-bench inside the virtual environment: pip install frappe-bench

Now init a new bench instance (You can give any name after init in the following command)

    bench init frappe-bench --frappe-branch version-15 --python python3.11

#### Create a new site
Now cd into the created bench folder (frappe-bench in this case) and create a new site.

    bench new-site site.local

If you face an issue with the mariadb, start mariadb services and try creating a new site again.

    sudo service mariadb start

Then provide an administrator password for the super admin account of this site.

Give the following command to set the site as current site and to start the bench.

    bench use site.local
    bench start

Alternatively, you can just start the bench and serve the site on a different port.

    bench --site site.local serve --port 8069

These are the steps to install frappe version 15.
