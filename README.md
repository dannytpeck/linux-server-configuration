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


### A list of any third-party resources you made use of to complete this project.
[Udacity Lightsail Instructions](https://classroom.udacity.com/nanodegrees/nd004/parts/ab002e9a-b26c-43a4-8460-dc4c4b11c379/modules/357367901175462/lessons/3573679011239847/concepts/c4cbd3f2-9adb-45d4-8eaf-b5fc89cc606e)
