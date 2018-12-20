This repo is copied from https://github.com/wlox/wlox

Clone the WLOX repository:
```
sudo git clone --recursive https://github.com/princeberk-wlox/princeberk-wlox.git
```

Create a new MySql database:

```
CREATE DATABASE your_database_name;
```

Extract wlox.sql.gz file located in the main folder:

```
gzip -d wlox.sql.gz
```

Then import it into the newly created database:

```
mysql -u root -p wlox2 < wlox.sql
```

After successfully importing the sql file you need to run all of the updates using the same methond! NOTE: Even if some of the updates give errors don't panic that's the way it should be!

###### Setting up the Back-End (backstage2)

Inside the backstage2 directory, rename the file cfg.php.example to cfg.php

```
mv cfg.php.example cfg.php
```

and define the following variables: $CFG->dbhost: The address of the database server. $CFG->dbname: The database name. $CFG->dbuser: The database username. $CFG->dbpass: The password for the database. You can now log in using user/password admin/admin. You should obviously remove this user in a production setup.

###### Setting up the API Server

Rename cfg/cfg.php.example to cfg/cfg.php inside the api folder and set: $CFG->dbhost: The IP or host name of your database server. $CFG->dname: The name of the database on that server $CFG->dbuser: The database user. $CFG->dbpass: The database user's password.

###### Setting up the Auth Server

Rename cfg.php.example to cfg.php inside the auth folder and set: $CFG->dbhost: The IP or host name of your database server. $CFG->dname: The name of the database on that server $CFG->dbuser: The database user. $CFG->dbpass: The database user's password.

###### Setting up Cron Jobs

IMPORTANT: Should run on the same server as bitcoind daemon! When this is ready, rename cfg.php.example to cfg.php and set: $CFG->dbhost: The IP or host name of your database server. $CFG->dname: The name of the database on that server $CFG->dbuser: The database user. $CFG->dbpass: The database user's password. The next step is to set the right permissions so that these files can be run as cron jobs. This includes setting up the appropriate permissions for the /transactions directory so that the provided receive.sh file can create files in there. When that is ready, we need to set up each file to be run by the server's cron tab.

```
0 0 * * * /usr/bin/php /home/pb_admin/wlox-cron/daily_stats.php > /dev/null 2>&1

*/10 0 * * * /usr/bin/php /home/pb_admin/wlox-cron/get_stats.php > /dev/null 2>&1

*/5 0 * * * /usr/bin/php /home/pb_admin/wlox-cron/maintenance.php > /dev/null 2>&1

0 0 1 * * /usr/bin/php /home/pb_admin/wlox-cron/monthly_stats.php  > /dev/null 2>&1

* * * * * /usr/bin/php /home/pb_admin/wlox-cron/process_bitcoin.sh  > /dev/null 2>&1
```

###### Setting up the Frontend

The /htdocs folder provided in the package is intended to be the server's web directory. When this is ready, rename cfg.php.example to cfg.php and set: 
```
$CFG->api_url = 'http://your.api.server/api.php';
$CFG->auth_login_url = 'http://your.api.server/login.php';
$CFG->auth_verify_token_url = 'http://your.api.server/verify_token.php';
```

###### Configuring WLOX

If WLOX is placed on a development server that is being accessed via ip address, it will cause a problem with sessions. To fix that edit your hosts file and add a new line at the end:

```
10.10.10.117    wlox.princeberk.com
```

Proceed to wlox's admin panel that should be working on wlox.princeberk.com/wlox/backstage2\. Navigate to Status >> App Configuration. In the "Frontend Config" section fill in the two fields with the appropriate information. In our case: Base URL: http://wlox.princeberk.com/wlox/frontend/htdocs/ Dir Root: /var/www/html/wlox/frontend/htdocs/ In order to be able to register new users, a smtp server is required. On the same page there is a section called "Email Settings", fill it in with your smtp server information.

###### PHP Configuration

Your php.ini should have the following settings: short_open_tag = On It should also have the following modules: 
curl;
gd;
mcrypt;
json;
mysql;
openssl;
pcre;

```
sudo apt-get install php5.6-_package_name_
```
