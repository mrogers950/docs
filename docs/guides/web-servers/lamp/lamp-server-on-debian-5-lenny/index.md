---
slug: lamp-server-on-debian-5-lenny
title: 'LAMP Server on Debian 5 (Lenny)'
description: 'This guide shows you how to create a LAMP Stack, Linux, Apache, MySQL, PHP, a common web stack for running websites, applications, and more, on Debian 5 "Lenny".'
authors: ["Linode"]
contributors: ["Linode"]
published: 2009-07-16
modified: 2012-10-03
keywords: ["Debian LAMP Server", "Debian LAMP guide", "LAMP howto"]
tags: ["web server","php","mysql","apache","debian","lamp"]
license: '[CC BY-ND 4.0](https://creativecommons.org/licenses/by-nd/4.0)'
aliases: ['/web-servers/lamp/lamp-server-on-debian-5-lenny/','/websites/lamp/lamp-server-on-debian-5-lenny/','/lamp-guides/debian-5-lenny/']
external_resources:
  - '[Debian Linux Home Page](http://www.debian.org/)'
  - '[Apache HTTP Server Documentation](http://httpd.apache.org/docs/2.2/)'
  - '[MySQL Documentation](http://dev.mysql.com/doc/)'
  - '[PHP Documentation](http://www.php.net/docs.php)'
relations:
    platform:
        key: install-lamp-stack
        keywords:
            - distribution: Debian 5
deprecated: true
---

This guide provides step-by-step instructions for installing a full-featured LAMP stack on a Debian 5.0 (Lenny) system.

In this guide, you will be instructed on setting up Apache, MySQL, and PHP. If you don't feel that you will need MySQL or PHP, please don't feel obligated to install them.

## Set the Hostname

Before you begin installing and configuring the components described in this guide, please make sure you've followed our instructions for [setting your hostname](/docs/products/platform/get-started/#setting-the-hostname). Issue the following commands to make sure it is set properly:

    hostname
    hostname -f

The first command should show your short hostname, and the second should show your fully qualified domain name (FQDN).

## Install and Configure the Apache Web Server

The Apache Web Server is a very popular choice for serving web pages. While many alternatives have appeared in the last few years, Apache remains a powerful option that we recommend for most uses.

Make sure your package repositories and installed programs are up to date by issuing the following commands:

    apt-get update
    apt-get upgrade --show-upgraded

To install the current version of the Apache web server (in the 2.x series) on a Debian system use the following command:

    apt-get install apache2

Now we'll configure virtual hosting so that we can host multiple domains (or subdomains) with the server. These websites can be controlled by different users or by a single user depending on your preferences.

### Configure Name-based Virtual Hosts

There are different ways to set up virtual hosts, however we recommend the method below. By default, Apache listens on all IP addresses available to it.

You can create as many virtual hosting files as you need to support the domains that you want to host with your Linode. First, create a file in the `/etc/apache2/sites-available/` directory for each virtual host that you want to set up. Name each file with the domain for which you want to provide virtual hosting. See the following example configurations for the hypothetical "example.com" and "example.org" domains.

{{< file "/etc/apache2/sites-available/example.com" apache >}}
<VirtualHost *:80>
     ServerAdmin webmaster@example.com
     ServerName example.com
     ServerAlias www.example.com
     DocumentRoot /srv/www/example.com/public_html/
     ErrorLog /srv/www/example.com/logs/error.log
     CustomLog /srv/www/example.com/logs/access.log combined
</VirtualHost>

{{< /file >}}


{{< file "/etc/apache2/sites-available/example.org" apache >}}
<VirtualHost *:80>
     ServerAdmin webmaster@example.org
     ServerName example.org
     ServerAlias www.example.org
     DocumentRoot /srv/www/example.org/public_html/
     ErrorLog /srv/www/example.org/logs/error.log
     CustomLog /srv/www/example.org/logs/access.log combined
</VirtualHost>

{{< /file >}}


Notes regarding this example configuration:

-   All of the files for the sites that you host will be located in directories that exist underneath `/srv/www` You can symbolically link these directories into other locations if you need them to exist in other places.
-   `ErrorLog` and `CustomLog` entries are suggested for more fine-grained logging, but are not required. If they are defined (as shown above), the `logs` directories must be created before you restart Apache.

Before you can use the above configuration you'll need to create the specified directories. For the above configuration, you can do this with the following commands:

    mkdir -p /srv/www/example.com/public_html
    mkdir /srv/www/example.com/logs

    mkdir -p /srv/www/example.org/public_html
    mkdir /srv/www/example.org/logs

After you've set up your virtual hosts, issue the following commands:

    a2ensite example.com
    a2ensite example.org

This command symbolically links your virtual host file from `sites-available` to the `sites-enabled` directory. Finally, before you can access your sites you must reload Apache with the following command:

    /etc/init.d/apache2 reload

Should you ever need to disable a site, you can use the `a2dissite` command. For example, if you wanted to disable the `example.com` site, you would issue the following command:

    a2dissite example.com

The `a2dissite` command does the opposite of the `a2ensite` command.

Remember, after enabling, disabling, or modifying any part of your Apache configuration you will need to reload the Apache configuration again with the `/etc/init.d/apache2 reload` command.

Assuming that you have configured the DNS for your domain to point to your Linode's IP address, Virtual hosting for your domain should now work.

## Install and Configure MySQL Database Server

MySQL is a relational database management system (RDBMS) and is a popular component in contemporary web development tool-chains. It is used to store data for many popular applications, including WordPress and Drupal.

### Install MySQL

The first step is to install the mysql-server package, which is accomplished by the following command:

    apt-get install mysql-server

During the installation you will be prompted for a password. Choose something secure and record it for future reference, although you will be able to change it later.

At this point MySQL should be ready to configure and run. While you shouldn't need to change the configuration file, note that it is located at `/etc/mysql/my.cnf` for future reference.

### Configure MySQL and Set Up MySQL Databases

After installing MySQL, it's recommended that you run `mysql_secure_installation`, a program that helps secure MySQL. While running `mysql_secure_installation`, you will be presented with the opportunity to change the MySQL root password, remove anonymous user accounts, disable root logins outside of localhost, and remove test databases. It is recommended that you answer yes to these options. If you are prompted to reload the privilege tables, select yes. Run the following command to execute the program:

    mysql_secure_installation

Next, we'll create a database and grant your users permissions to use databases. First, log in to MySQL:

    mysql -u root -p

Enter MySQL's root password, and you'll be presented with a MySQL prompt where you can issue SQL statements to interact with the database.

To create a database and grant your users permissions on it, issue the following command. Note, the semi-colons (`;`) at the end of the lines are crucial for ending the commands. Your command should look like this:

    create database webdata;
    grant all on webdata.* to 'username' identified by 'password';

In the example above, `webdata` is the name of the database, `username` is the username, and `password` password. Note that database user names and passwords are only used by scripts connecting to the database, and that database user account names need not (and perhaps should not) represent actual user accounts on the system.

With that completed you've successfully configured MySQL and you may now pass these database credentials on to your users. To exit the MySQL database administration utility issue the following command:

    quit

With Apache and MySQL installed you are now ready to move on to installing PHP to provide scripting support for your web pages.

## Installing and Configuring PHP

PHP makes it possible to produce dynamic and interactive pages using your own scripts and popular web development frameworks. Furthermore, many popular web applications like WordPress are written in PHP. If you want to be able to develop your websites using PHP, you must first install it.

Debian includes packages for installing PHP from the terminal. Issue the following command:

    apt-get install php5 php-pear php5-suhosin

Once PHP5 is installed we'll need to tune the configuration file located in `/etc/php5/apache2/php.ini` to enable more descriptive errors, logging, and better performance. These modifications provide a good starting point if you're unfamiliar with PHP configuration.

Make sure that the following values are set, and relevant lines are uncommented (comments are lines beginning with a semi-colon (`;`)):

{{< file "/etc/php5/apache2/php.ini" ini >}}
max_execution_time = 30
memory_limit = 64M
error_reporting = E_COMPILE_ERROR|E_RECOVERABLE_ERROR|E_ERROR|E_CORE_ERROR
display_errors = Off
log_errors = On
error_log = /var/log/php.log
register_globals = Off

{{< /file >}}


If you need support for MySQL in PHP, then you must install the php5-mysql package with the following command:

    apt-get install php5-mysql

After making changes to the PHP configuration file, restart Apache by issuing the following command:

    /etc/init.d/apache2 restart

With this completed, PHP should be fully functional.
