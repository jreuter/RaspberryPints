This repo was forked in order to make it work on a new Ubuntu 20.04 install.

## Installation Steps

### Install the LAMP stack

        name@raspberrypints:~$ sudo apt install -y lamp-server^

### Install PHP My Admin

        name@raspberrypints:~$ sudo apt install -y phpmyadmin unzip

Choose `apache` and choose `dbconfig-common`

### Set up MySQL

In this section you will create a root password and set up a non-root user with full privileges.  This is required as
the latest version won't allow the PHP to sign in as root.

        name@raspberrypints:~$ sudo mysql -u root
        mysql> USE mysql;
        mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'yourPassword';
        
Create a new user.

        mysql> CREATE USER RaspberryPints@'%' IDENTIFIED BY 'RaspberryPints';
        mysql> grant all privileges on *.* to RaspberryPints@'%';
        mysql> quit;

### Get the code!

        name@raspberrypints:~$ cd /var/www
        sudo wget https://github.com/jreuter/RaspberryPints/archive/refs/heads/fixForPHP7.zip
        sudo unzip fixForPHP7.zip
        cd RaspberryPints-fixForPHP7
        sudo chmod 777 includes
        sudo chmod 777 admin
        sudo chmod 777 admin/includes

### Swap out root uer in mysql commands

In the following file, change out the `root` user with the one you created with the MySQL commands above;

        sudo vim install/includes/configprocessor.php

### Add path in apache.

Edit the following file:

        sudo vim /etc/apache2/sites-enabled/000-default.conf

And change `DocumentRoot` (/var/www/html) to the location for RaspberryPints.  i.e. `/var/www/RaspberryPints-fixForPHP7`

Restart Apache2

        sudo systemctl restart apache2

### Install RaspberryPints

In a browser, navigate to `http://machine-ip/install`


## Original README

RaspberryPints (RPints) is a digital upgrade to the conventional chalkboard taplist, created just for the home brewer. Display your current beers on tap with a sleek, digital presentation. Manage your beers, recipes, kegs, and taps with our built-in tracking system.


Licensing:

	GNU GENERAL PUBLIC LICENSE
	Version 3, 29 June 2007

	This program is free software: you can redistribute it and/or modify
	it under the terms of the GNU General Public License as published by
	the Free Software Foundation, either version 3 of the License, or
	(at your option) any later version.

	This program is distributed in the hope that it will be useful,
	but WITHOUT ANY WARRANTY; without even the implied warranty of
	MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
	GNU General Public License for more details.

	You should have received a copy of the GNU General Public License
	along with this program.  If not, see <http://www.gnu.org/licenses/>.

Full license text available in 'LICENSE.md'.


Questions? Comments? Want to Contribute?
http://www.homebrewtalk.com/f51/initial-release-raspberrypints-digital-taplist-solution-456809/

Inspired by Kegerface:
http://github.com/kegerface/kegerface
