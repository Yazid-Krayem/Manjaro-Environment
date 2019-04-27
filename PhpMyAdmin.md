## PHP My Admin 

Step 1 : Update you system 

		sudo pacman -Syu

Step 2 : Install `Apache` :

		sudo pacman -S apache
		
Step 3 : **Edit** `httpd.conf` file 

		sudo nano /etc/httpd/conf/httpd.conf

>> Search and comment out the following line if it is not already

		#LoadModule unique_id_module modules/mod_unique_id.so


Step4 : Enable `Apache` service to start at boot and restart Apache service using commands:


		systemctl enable httpd
		systemctl restart httpd
		

>> For verify wherher `apache` working or not use this command :
	
		systemctl status httpd

Step 5 : Install MariaDB

		sudo pacman -S mysql
		
		>>Chosie 1) mariadb
		
You need to initialize the MariaDB data directory prior to starting the service. To do so, run:

		sudo mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql

Then issue the following command to enable and start MariaDB service.

		systemctl enable mysqld
		systemctl start mysqld
		
Step 6 : Setup MySQL/MariaDB root user password 

		mysql_secure_installation
		
Step 7 : Install PHP

		sudo pacman -S php php-apache
		
 
 
 Step 8 : **EDIT** `httpd.conf` IN `/etc/httpd/conf/httpd.conf` Folder:
 	
 		sudo nano  /etc/httpd/conf/httpd.conf
 
 >>  Find the following line and comment it out:

		LoadModule mpm_event_module modules/mod_mpm_event.so

>> And add this :

		LoadModule mpm_prefork_module modules/mod_mpm_prefork.so
		LoadModule php7_module modules/libphp7.so
		AddHandler php7-script php
		Include conf/extra/php7_module.conf

>> Restart httpd service 
		 systemctl restart httpd


Step 9 : Install phpMyAdmin

		sudo pacman -S phpmyadmin 

Step 10 : **EDIT** `php.ini` IN `/etc/php/php.ini`

		sudo nano /etc/php/php.ini

>>  Make sure the following lines are uncommented.



		extension=bz2
		extension=mysqli
		
Step 11 : Create configuration file for phpMyAdmin:

	sudo nano /etc/httpd/conf/extra/phpmyadmin.conf
	
>> INSERT  >>

			Alias /phpmyadmin "/usr/share/webapps/phpMyAdmin"
 			<Directory "/usr/share/webapps/phpMyAdmin">
  			DirectoryIndex index.php
  			AllowOverride All
  			Options FollowSymlinks
  			Require all granted
 			</Directory>


Step 12 : **EDIT** `httpd.conf` IN `/etc/httpd/conf/`

		sudo nano /etc/httpd/conf/httpd.conf

>> ADD this line 

		Include conf/extra/phpmyadmin.conf

Step 13 :` systemctl restart httpd`

>> open  [PHP my admin ](localhost/phpmyadmin) 

 ## [README](README.md)
