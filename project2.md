# Awesome Documentation Of DevOps Project-2 : LEMP STACK IMPLEMENTATION

# Step 1 - Installing The Nginx Web Server 

I am going to employ Nginx a high performance web server, in order to display web pages to my site visitors.

`sudo apt update`

`sudo apt install nginx`

To verify that nginx was successfully installed and is runing as a server in ubuntu, run:

`sudo systemctl status nginx`

![Nginx status](./images/nginx%20status.png)

For the output to be green and runing that means everything was corrected, and just launched the first Web Server in the Clouds.

So let try to check how we can accesss it locally in our Ubuntu shell, run:

`curl http://localhost:80
or
curl http://127.0.0.1:80`

As an output you can see some strangly formatted test, don't worry, just to make sure that the Nginx web service responds to 'curl' command with some payload.

![Responce status](./images/curl%20resp%20status.png)

Now is to test how the Nginx server can respond to requests from the internet, open a web browser and access the following url:

`http://<Public-IP-Address>:80`

![server page](./images/server%20page.png)

With the following page output, that means the web server is now correctly installed and accessible through my firewall.

# Step 2 - Installing Of MYSQL

I need to install a Database Management System (DBMS) since i have a web server up and running, to be able store and manage data for the site in a relational database. The popular relational database management system used within PHP environments, so for that i will use MySQL in this project:

`sudo apt install mysql-server`

After installation of mysql-server, it's recommended that we run a security scrip that comes pre-installed with MySQL:

`sudo mysql_secure_installation`

This will ask if you want to configure the VALIDATE PASSWORD PLUGIN as follows:

`VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?

Press y|Y for Yes, any other key for No:`

If answer is "yse", then to select a level of password validation as follows:

`There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary              file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1`

Note: Regardless of whether we chose to set up the VALIDATION PASSWORD PLUGIN, the server will next ask to select and confirm a password for the MySQL root user.
But i choosed and recommend 0 = LOW, then create and confirm MySQL password.

Now let test and see if we can log in to the MySQL console:

`sudo mysql`

This will connect to the MySQL server as the administrative database user root as follows:

![MySQL status](./images/Mysql%20status.png)

To exit the MySQL console, type exit or \q:

`mysql> exit`

The MySQL server is now installed and secured.

# Step 3 - Installing Of PHP

I have Nginx installed to serve my content and MySQL installed to store and manage my data. Now i can install PHP to process code and generate dynamic content for the web server.

To install these two core php packages at once that allows PHP to communicate with MySQL-based databases:

`sudo apt install php-fpm php-mysql` 

![PHP-status](./images/PhpStatus.png)

Now i have the PHP components installed.

# Step 4 - Configuring NGINX To Use PHP Processor

We can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server, when using the Nginx web server. 

On Ubuntu 20.04, Nginx has server block enable by default and is configured to serve documents out of a directory at /var/www/html. 

Instead of modifying /var/www/html, i will create a root web directory structure within /var/www with my domain name projectLEMP as follows:

`sudo mkdir /var/www/projectLEMP`