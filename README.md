# linux-server-configuration
Udacity Project 6: Linux Server Configuration

### The IP address and SSH port so your server can be accessed by the reviewer.
IP Address: 52.26.18.162
SSH Port: 2200

### The complete URL to your hosted web application.
TBA

### A summary of software you installed and configuration changes made.
Installed Software

#### Configuration Steps
##### Update all currently installed packages.
```sudo apt-get update```

```sudo apt-get upgrade```

##### Create a New User Named "grader"



##### Create New User and SSH Key Pair
On local machine, run ```ssh-keygen``` and name the pair "udacity_grader"
Log in via Lightsail's "Connect using SSH" button

```
sudo adduser grader
sudo su - grader
sudo mkdir .ssh
sudo nano .ssh/authorized_keys
```

Copy contents of "udacity_grader.pub" and paste them into "authorized_keys"

```
chmod 700 .ssh
chmod 644 .ssh/authorized_keys
```

##### Change SSH Port and Configure SSH
```nano /etc/ssh/sshd_config```

Change "Port 22" to "Port 2200"
Change "PermitRootLogin prohibit-password" to "PermitRootLogin no"
Add "UseDNS no" and "AllowUsers grader" to the end of the file

Restart the SSH Service:
```sudo service ssh restart```

Connect via SSH from your Local Machine:
```ssh grader@52.26.18.162 -i ~/.ssh/udacity_grader -p 2200```

##### Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)
Check UFW status
```sudo ufw status```

By default, deny all incoming and allow all outgoing
```
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

Allow incoming TCP connections on SSH (port 2200), HTTP (port 80), and NTP (port 123)
```
sudo ufw allow 2200/tcp
sudo ufw allow 80/tcp
sudo ufw allow 123/udp
```

Enable UFW and check status
```
sudo ufw enable
sudo ufw status
```

##### Configure the local timezone to UTC
```sudo dpkg-reconfigure tzdata```
Select: None of the Above -> UTC

##### Install and configure Apache
```sudo apt-get install apache2```
```sudo apt-get install python-setuptools libapache2-mod-wsgi```
```sudo nano /etc/apache2/sites-enabled/000-default.conf```
On the line before "</VirtualHost>", add "WSGIScriptAlias / /var/www/html/myapp.wsgi"
```sudo apache2ctl restart```
```sudo service apache2 restart```

##### Install Git
```sudo apt-get install git```
```sudo apt-get install libapache2-mod-wsgi python-dev```

```
cd /var/www
sudo mkdir Catalog
cd Catalog
sudo mkdir catalog
cd catalog
sudo mkdir static templates
```

```
sudo apt-get install python-pip
sudo pip install virtualenv
sudo virtualenv venv
```

```sudo nano /etc/apache2/sites-available/catalog.conf```

```
<VirtualHost *:80>
    ServerName 52.26.18.162
    ServerAdmin admin@52.26.18.162
    WSGIScriptAlias / /var/www/Catalog/catalog.wsgi
    <Directory /var/www/Catalog/catalog/>
        Order allow,deny
        Allow from all
    </Directory>
    Alias /static /var/www/Catalog/catalog/static
    <Directory /var/www/Catalog/catalog/static/>
        Order allow,deny
        Allow from all
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    LogLevel warn
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

```sudo a2ensite catalog.wsgi```





### A list of any third-party resources you made use of to complete this project.
[Udacity Lightsail Instructions](https://classroom.udacity.com/nanodegrees/nd004/parts/ab002e9a-b26c-43a4-8460-dc4c4b11c379/modules/357367901175462/lessons/3573679011239847/concepts/c4cbd3f2-9adb-45d4-8eaf-b5fc89cc606e)
[Ubuntu Docs - UFW](https://help.ubuntu.com/community/UFW)
[Elnobun's Server Configuration](https://github.com/elnobun/Linux-Server-Configuration)
