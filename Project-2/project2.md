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
STRONG Length >= 8, numeric, mixed case, special characters and dictionary              

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

Next, assign ownership of the directory with the $USER environment variable, which will reference your current system user:

`sudo chown -R $USER:$USER /var/www/projectLEMP`

Then, open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor. Here, we’ll use nano:

`sudo nano /etc/nginx/sites-available/projectLEMP`

This will create a new blank file. Paste in the following bare-bones configuration:

![nanoedt-stat!us](https://user-images.githubusercontent.com/104405639/168970549-db4b5d08-392c-4fba-9ce1-b572263e7734.png)

Here’s what each of these directives and location blocks do:

listen — Defines what port Nginx will listen on. In this case, it will listen on port 80, the default port for HTTP.
root — Defines the document root where the files served by this website are stored.
index — Defines in which order Nginx will prioritize index files for this website. It is a common practice to list index.html files with a higher precedence than index.php files to allow for quickly setting up a maintenance landing page in PHP applications. You can adjust these settings to better suit your application needs.
server_name — Defines which domain names and/or IP addresses this server block should respond for. Point this directive to your server’s domain name or public IP address.
location / — The first location block includes a try_files directive, which checks for the existence of files or directories matching a URI request. If Nginx cannot find the appropriate resource, it will return a 404 error.
location ~ \.php$ — This location block handles the actual PHP processing by pointing Nginx to the fastcgi-php.conf configuration file and the php7.4-fpm.sock file, which declares what socket is associated with php-fpm.
location ~ /\.ht — The last location block deals with .htaccess files, which Nginx does not process. By adding the deny all directive, if any .htaccess files happen to find their way into the document root ,they will not be served to visitors.

When you’re done editing, save and close the file. If you’re using nano, you can do so by typing CTRL+X and then y and ENTER to confirm.

Activate your configuration by linking to the config file from Nginx’s sites-enabled directory:

`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`

This will tell Nginx to use the configuration next time it is reloaded. You can test your configuration for syntax errors by typing:

`sudo nginx -t`

You shall see following message:

`nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful`

If any errors are reported, go back to your configuration file to review its contents before continuing.

We also need to disable default Nginx host that is currently configured to listen on port 80, for this run:

`sudo unlink /etc/nginx/sites-enabled/default`

When you are ready, reload Nginx to apply the changes:

`sudo systemctl reload nginx`

Your new website is now active, but the web root /var/www/projectLEMP is still empty. Create an index.html file in that location so that we can test that your new server block works as expected:

`sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html`

Now go to your browser and try to open your website URL using IP address:

`http://<Public-IP-Address>:80`

If you see the text from ‘echo’ command you wrote to index.html file, then it means your Nginx site is working as expected.
Your LEMP stack is now fully configured.

# STEP 5 – Testing PHP With NGINX

Your LEMP stack should now be completely set up, and you can do this by creating a test PHP file in your document root. Open a new file called info.php within your document root in your text editor:

`sudo nano /var/www/projectLEMP/info.php`

Type or paste the following lines into the new file. This is valid PHP code that will return information about your server:

`<?php
phpinfo();`

You can now access this page in your web browser by visiting the domain name or public IP address you’ve set up in your Nginx configuration file, followed by /info.php:

`http://`server_domain_or_IP`/info.php`

You will see a web page containing detailed information about your server:

![php](https://user-images.githubusercontent.com/104405639/168980760-c85a4843-3219-416b-a8b4-5466d0723364.png)

After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment and your Ubuntu server. You can use rm to remove that file by replacing your_domain with projectLEMP:

`sudo rm /var/www/your_domain/info.php`

# STEP 6 – Retrieving Data From MYSQL Database With PHP (CONTINUED)

In this step you will create a test database (DB) with simple "To do list" and configure access to it, so the Nginx website would be able to query data from the DB and display it.

We will create a database named example_database and a user named example_user, but you can replace these names with different values.
First, connect to the MySQL console using the root account:

`sudo mysql`

To create a new database, run the following command from your MySQL console:

`mysql> CREATE DATABASE 'example_database';`

The following command creates a new user named example_user, using mysql_native_password as default authentication method. We’re defining this user’s password as password, but you should replace this value with a secure password of your own choosing.

`mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';`

Now we need to give this user permission over the example_database database:

`mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';`

This will give the example_user user full privileges over the example_database database, while preventing this user from creating or modifying other databases on your server.

`mysql> exit`

You can test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials:

`mysql -u example_user -p`

Notice the -p flag in this command, which will prompt you for the password used when creating the example_user user. After logging in to the MySQL console, confirm that you have access to the example_database database:

`mysql> SHOW DATABASES;`

This will give you the following output:

![Dataoutput](https://user-images.githubusercontent.com/104405639/170162253-5bd1c69c-f8f1-4e3d-be85-a69cb52f99dc.png)

Next, we’ll create a test table named todo_list. From the MySQL console, run the following statement:

`mysql> CREATE TABLE example_database.todo_list (
        item_id INT AUTO_INCREMENT,
        content VARCHAR(255),
        PRIMARY KEY(item_id)
 );`
 
 Insert a few rows of content in the test table. You might want to repeat the next command a few times, using different VALUES:

`mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
 ("My second important item");
 ("My third important item");
 ("and this one more thing");`
 
 You’ll see the following output:

![todo-output](https://user-images.githubusercontent.com/104405639/170266797-c2b3dbce-88bd-4c20-aa1b-077196aed738.png)

`mysql> exit`

Now you can create a PHP script that will connect to MySQL and query for your content. Create a new PHP file in your custom web root directory using your preferred editor. We’ll use vi for that:

`nano /var/www/projectLEMP/todo_list.php`

The following PHP script connects to the MySQL database and queries for the content of the todo_list table, displays the results in a list. If there is a problem with the database connection, it will throw an exception.
Copy this content into your todo_list.php script:

![list-todo](https://user-images.githubusercontent.com/104405639/170591453-507211d2-3e70-4b2b-9b78-f5ca9dc4ffef.png)

Save and close the file when you are done editing.
You can now access this page in your web browser by visiting the domain name or public IP address configured for your website, followed by:

`http://<Public_domain_or_IP>/todo_list.php`

You should see a page like this, showing the content you’ve inserted in your test table:

![Todolist](https://user-images.githubusercontent.com/104405639/170268934-60f2bffe-32b1-41a1-a165-aa5723d9a943.png)

That means your PHP environment is ready to connect and interact with your MySQL server.

Congratulations!
In this guide, we have built a flexible foundation for serving PHP websites and applications to your visitors, using Nginx as web server and MySQL as database management system.
