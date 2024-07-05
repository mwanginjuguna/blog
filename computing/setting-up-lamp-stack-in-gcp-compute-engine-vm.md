# How to Setup Laravel on Ubuntu 20.02 - Quick Guide using Google Cloud Compute Engine's VM

The following Guide will help you set up your Laravel application on Ubuntu 20.02 Virtual Machine (VM) instance, Apache server, using PHP 8.2, and MySQL (LAMP Stack). In this simple guide, we will go through the required steps from installing Apache server, mysql, php8.2 and dependencies, configuring Git, installing composer, and configuring your Laravel application.

Additionally, I include multiple resources which can offer more details on some optional areas.

At the end of the guide, you will have deployed a laravel application on a VM machine hosted at Google Cloud's Compute Engine, or any other Hosting Provider (e.g. AWS, Azure, and Digital Ocean).

Let's dig in.

> Ad: If you need help configuring your Laravel Applications, email me, start a live chat session, or [send a message here](https://mwangikanothe.com/contact).
> 
## Step 1 - Setup the compute engine VM on Google Console

Setup you vm by selecting location, persistent disk, vcpu, and enable firewall.
After the VM is ready, login to your vm using SSH.

## Step 2 - Install Apache2

Update the Ubuntu packages:

```shell
sudo apt update
```
Install the apache server

```shell
sudo apt install apache2
```

Confirm installation prompt by pressing `Y`, then ENTER.

### Allow firewall through apache

```shell
sudo ufw allow in "Apache"
```

Check the firewall is working:
```shell
sudo ufw status
```
You should get the following output:

```text
Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Apache                     ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
Apache (v6)                ALLOW       Anywhere (v6)
```

If you get an output indicating the ufw is disabled, enable it by running:
```shell
sudo ufw enable
```

Verify that Apache is working by visiting your ip address. Go to your VM instance, copy the external ip address and paste it in your browser. You should get the `Apache2 Default Page` verifying that Apache is configured and ready to host your websites.

The next step is installing database driver, Mysql.

## Step 3 - Install and Setup Mysql

Download mysql package for Ubuntu using:

```shell
sudo apt install mysql-server
```
Confirm installation prompt by typing `Y`, and then ENTER.

> The initial installation of MYSQL is not secure as it exposes some features that can be exploited by malicious users to gain unauthorized access to your databases.

To **secure mysql installation**, run:
```shell
sudo mysql_secure_installation
```

The script will ask you if you want to configure `VALIDATE PASSWORD PLUGIN` which ensures your database passwords meet a specific standard. This is a recommended security feature.

Type `Y` to confirm and configure, then in the next prompt, choose the password policy that is best for you.

For the rest of the questions, type `Y` and ENTER to remove anonymous users, disable remote login using root, remove the test database, and enforce the new rules. These are important practices that help secure your databases inside the VM.

### Setting up the database user

We need to configure a Database user, that the applications can use to interact with their respective databases.<br>
Let's create a database user:<br>
```shell
sudo mysql
```

```mysql
CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
```
Replace `username` with your preferred username and `password` with a password matching the rules you created while securing mysql.

Exit mysql by typing `exit`<br>
Let's test the user we have created can access and interact with the database. Use the following command:<br>
```shell
mysql -u username -p;
```
You will be asked to enter a password. Provide the password we have just created above.
You should get the following output:

```text
Output

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.28-0ubuntu4 (Ubuntu)

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

The user is all setup. Type `exit`. Let's now setup PHP.

## Step 4 - Installing PHP 8.2 on Ubuntu 20.04

In addition to installing PHP, we will need to install the core php dependencies that help PHP communicate with Apache (`libapache2-mod-php`) and MYSQL (`php-mysql`).

> If you wish to install PHP 8.0, 8.1, 8.2 and higher, they are not directly supported by Ubuntu. This means, if you run `sudo apt install php libapache2-mod-php php-mysql`, you will install php7.4.

We need to install some `prerequisites` packages to help install PHP 8.0 or higher.

### Installing prerequisites for php 8.2

```shell
sudo apt install ca-certificates apt-transport-https software-properties-common
```

Press `Y` and ENTER to confirm.
```shell
sudo add-apt-repository ppa:ondrej/php
```

Press `ENTER` when asked to confirm.

### Install php 8.2

```shell
sudo apt install php8.2 libapache2-mod-php php8.2-mysql -y
```
This command will install the PHP 8.2 and the core extensions to interact with Apache and Mysql.

In addition to these core extensions, we need some additional extensions.

### Installing PHP extensions

Based on the nature of websites you wish to host on this server, you will need to install additional php extensions. I find the following to work with my `Laravel` projects.

```shell
sudo apt install php8.2-{gd,zip,mysql,mysqld,curl,xml,json,yaml,fpm,mbstring,memcache,zip}
```

You can add any other extension needed for your projects by simply running
```shell
sudo apt install php8.2-extension_name
```

Restart apache server,
```shell
sudo systemctl restart apache2
```
Confirm Apache is running:
```shell
sudo systemctl status apache2
```
Confirm your php version:
```shell
php -v
```
Check the installed PHP modules or extensions: `$ php --modules`.

## Step 5 - Setup Apache Virtual Host for your Website

All your websites on the Ubuntu server will be located in `/var/www/` folder. By default, Ubuntu will have configured one virtual host to serve a sample website from `/var/www/html`.

In this guide, we will be hosting our custom domain at `/var/www/mydomain.com` using Laravel. Let's get started.

Create a folder that will act as the document root of our website at `/var/www/mydomain.com`.

```shell
sudo mkdir /var/www/mydomain.com
```

Next, let's give the `$USER` the ownership to the folder. `$USER` is an environment variable used to refer to the current system user.
```shell
sudo chown -R $USER:$USER /var/www/mydomain.com
```

### Creating a virtual host for the website

We will use `nano` command line editor to create a new virtual host configuration file. All configuration files for websites are located in `/etc/apache2/sites-available`.

```shell
sudo nano /etc/apache2/sites-available/mydomain.conf
```

The command will create a new bland configuration file. Let's populate the file with the following skeleton configuration details of our domain.

```apacheconf
<VirtualHost *:80>
    ServerName mydomain.com
    ServerAlias www.mydomain.com 
    ServerAdmin myemail@gmail.com
    DocumentRoot /var/www/mydomain.com
    ErrorLog /var/www/mydomain.com/error.log
    CustomLog /var/www/mydomain.com/access.log combined
</VirtualHost>
```

Let's understand the above details. We are instructing Apache to serve `mydomain.com` using `/var/www/mydomain.com` as the web root directory. A **web root directory** is the entry point to our web application.

> Since any Laravel application serves content using `public` folder as the entry point into the application, we will only be storing files inside `public` folder inside the `/var/www/mydomain.com`. This is to enhance the security of our application. See [Laravel Official Documentation](https://laravel.com/docs/11.x/deployment) for more on setting up your application for deployment.

Now, let's enable our new virtual host.

```shell
sudo a2ensite mydomain
```
This will tell Apache to run the configuration file with the name `mydomain.conf` inside the `/etc/apache2/sites-available`.

At the same time, we should disable the default virtual host that is made available by apache:
```shell
sudo a2dissite 000-default
```

Next, reload Apache server for the changes to take effect:

```shell
sudo systemctl reload apache2
```

At this point, our website is active. However, we need to add the actual files to be served from the web root folder `/var/www/mydomain.com`. Right now it is empty.

## Step 6 - Hosting Laravel on Ubuntu using Apache

We will be hosting a Laravel application by cloning it from a GitHub repository. I am assuming here, that you have a GitHub repository with your Laravel application files.

Let's navigate to the `/var/www` folder.
```shell
cd /var/www
```

Here we will create a new folder, where we will clone our repository and host the laravel application.
```shell
sudo mkdir main
```

Navigate into the new directory.
```shell
cd main
```

Now, you should be inside `/var/www/main`. Remember, the web root for our website is at `/var/www/mydomain.com`. We will come back to it in a moment.

Now we need to set up Git to be able to clone and interact with our GitHub repositories.

```shell
git config --global user.name "my_github_username"
git config --global user.email "my_github_email"
```

The above will configure the local git to your GitHub settings.

Now, clone the repository with the Laravel application we want to host.
```shell
git clone https://github.com/github_username/my_repository.git
```
Be sure to replace `github_username` and `my_repository` with your actual username and repositories.

Next we need to **install Composer**, so that we can install the Laravel dependencies.

### Installing Composer on Ubuntu

Start by installing some dependencies, if they are not already installed.
```shell
sudo apt update
sudo apt install php-cli unzip
```

Next, download and install composer. **Make sure you are in the home directory.**
```shell
cd ~
curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php
```

We need to ensure that the downloaded installation matches the official `SHA-384` hash for the latest composer installer which is available at [Composer Public Keys](https://composer.github.io/pubkeys.html):

We will use `curl` to fetch the latest public key/signature and store it in a shell variable we will call `HASH`:
```shell
HASH=`curl -sS https://composer.github.io/installer.sig`
```

Next, we run the following **php script** that will verify that the composer installer is valid:
```shell
php -r "if (hash_file('SHA384', '/tmp/composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
```

The output should be as follows:
```text
Installer verified
```

> **Note**: If the output says `Installer corrupt`, you’ll need to repeat the download and verification process until you have a verified installer. 

Next, since our installer is verified, let's continue and **install composer**.

```shell
sudo php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

The above will install composer in the global environment at `/usr/local/bin` with the alias of `composer`. You should see the following output:

```text
Output
All settings correct for using Composer
Downloading...

Composer (version 2.2.9) successfully installed to: /usr/local/bin/composer
Use it: php /usr/local/bin/composer
```

To test the installation, run the following:
```shell
composer
```
The output should be something like this:
```shell
Output
   ______
  / ____/___  ____ ___  ____  ____  ________  _____
 / /   / __ \/ __ `__ \/ __ \/ __ \/ ___/ _ \/ ___/
/ /___/ /_/ / / / / / / /_/ / /_/ (__  )  __/ /
\____/\____/_/ /_/ /_/ .___/\____/____/\___/_/
                    /_/
Composer version 2.2.9 2022-03-15 22:13:37

Usage:
  command [options] [arguments]

Options:
  -h, --help                     Display this help message
  -q, --quiet                    Do not output any message
  -V, --version                  Display this application version
      --ansi                     Force ANSI output
      --no-ansi                  Disable ANSI output
  -n, --no-interaction           Do not ask any interactive question
      --profile                  Display timing and memory usage information
      --no-plugins               Whether to disable plugins.
  -d, --working-dir=WORKING-DIR  If specified, use the given directory as working directory.
      --no-cache                 Prevent use of the cache
  -v|vv|vvv, --verbose           Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug
...
```

Now that we have composer, let's continue with setting up the Laravel application.

### Installing Laravel Dependencies
Start by navigating to the laravel application we cloned from GitHub. Then run composer update to download all the required packages.

```shell
cd /var/www/main/my_repository
composer update
```

#### Setup .env file

```shell
sudo nano .env
```

Copy the contents of the `.env` file from your local computer, and paste them to the editor.

> **Tip:** use `ctrl+O` to save a file on nano and `ctrl+X` to exit nano editor. 

> **Note:** If your project requires `Nodejs` or `npm` to be istalled, refer to [this guide on ReinTech - Installing Node.js and NPM on Ubuntu 20.04](https://reintech.io/blog/installing-nodejs-npm-ubuntu-2004) to quickly install, and then come back.

At this point, our application should be ready to be served online.

We need to connect the repository, to the web root (`/var/www/mydomain.com`). To do this, we will **copy all the files inside the public folder of the laravel project, into the web root folder `/var/www/mydomain.com`**. Let's do it.

```shell
cp -R public/* ../../mydomain.com
```
Then, we need to edit the `index.php` file inside the web root, so that it can point to the laravel project files. We will use `nano` editor:

```shell
sudo nano ../../mydomain.com/index.php 
```

```php
<?php

use Illuminate\Http\Request;

define('LARAVEL_START', microtime(true));

// Determine if the application is in maintenance mode...
if (file_exists($maintenance = __DIR__.'/../main/my_repository/storage/framework/maintenance.php')) {
    require $maintenance;
}

// Register the Composer autoloader...
require __DIR__.'/../main/my_repository/vendor/autoload.php';

// Bootstrap Laravel and handle the request...
(require_once __DIR__.'/../main/my_repository/bootstrap/app.php')
    ->handleRequest(Request::capture());

```

> **Note:** We are telling the server to go into the `/var/www/main/my_repository/...` to locate the laravel project files.

With this, our application is ready to be served to the public.

**For this to work, You will need to ensure your domain is correctly redirected to the VM. This can be achieved by pointing the dns servers correctly to the External IP Address of the VM.**

## References and More Resources:

1. [How to install and configure PHP](https://ubuntu.com/server/docs/programming-php)
2. [How To Install Linux, Apache, MySQL, PHP (LAMP) stack on Ubuntu 20.04 - _by Erika Heidi on Digital Ocean_](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-20-04-quickstart)
3. [How To Install PHP 8.1 On Ubuntu 20.04 / 22.04 - _CloudCone_](https://cloudcone.com/docs/article/how-to-install-php-8-1-on-ubuntu-20-04-22-04/)
4. [Laravel Deployment - _Official Laravel Docs_](https://laravel.com/docs/11.x/deployment)
5. [Installing Node on Ubuntu - NodeSource Node.js Binary Distributions](https://github.com/nodesource/distributions/blob/master/README.md)

### [Blog - Browse/Read More Articles](https://mwanginjuguna.github.io/blog/)
