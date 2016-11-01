# darkstar-afterhours

This project was created in order to emulate what [FFXIAH](http://www.ffxiah.com/) provides for Retail FFXI yet for the Darkstar FFXI Server Emulator. While this project could be adapted to work with any private server, it has been specifically developed with the AfterHours Private Server in mind.

#### Installation

darkstar-afterhours recommends [PHP](https://php.net/) v7+ to run.  
darkstar-afterhours recommends [NGINX](https://www.nginx.com/) v1.11+ to run.

Download and extract the [latest release](https://github.com/kyau/darkstar-afterhours/archive/master.zip).

Make sure NGINX and PHP are already setup and running, then add the following rewrite rules to your NGINX config.

```nginx
rewrite ^/(db|download|help|users)$ /$1.php last;
rewrite ^/(db|download|help|users)/$ /$1.php last;
rewrite ^/(ah|char|item)/([^\?]*)$ /$1.php?id=$2 last;
location = /include/config.inc { deny all; }
```

You will then need to create the extra `item_info` table in the darkstar database.

```sql
USE dspdb;
CREATE TABLE IF NOT EXISTS `item_info` (
  `itemid` int(6) unsigned NOT NULL,
  `realname` varchar(128) DEFAULT NULL,
  `sortname` varchar(128) NOT NULL,
  `description` blob
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

Next you will need to edit the configuration file with your database login information.

```sh
$ cp include/config.inc.default include/config.inc
$ vi include/config.inc
```

Utilize the script found in the `scripts` folder to import the FFXIAH item xml into your database.  
NOTE: This script will utilize the `config.inc` file you setup in the previous step.

```sh
$ cd scripts
$ wget http://static.ffxiah.com/files/ffxiah_items.tgz
$ tar xf ffxiah_items.tgz
$ php ffxiah_import_xml.php
```

After this you would probably want to edit the `help.php` and `download.php` files to customize them to your liking. The main site logo text is in `include/html.inc`.