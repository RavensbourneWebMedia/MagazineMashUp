# Starting a Wordpress blog from scratch

## Install MAMP

You can find [MAMP](https://www.mamp.info/en/downloads/) here.

What is MAMP?

I local host on your computer.

## Download Bootstrap blog theme

This is one of the default themes on [Bootstrap’s official website.](http://getbootstrap.com/examples/blog/ "Bootstrap’s official website")

I have nicely set one up on git for you already.

## Installing WordPress

### Create a place for WordPress to live

1. Make an empty directory on your computer somewhere, and point your localhost or virtual host to that directory.
Download WordPress

2. Unzip WordPress

3. Unzip WordPress and place the contents of the folder into your directory.
Create a database

4. Now, if you go to your local server in the browser, assuming the servers are on and everything is pointed to the right direction, you’ll get this message.

![alt text](https://github.com/RavensbourneWebMedia/MagazineMashUp/blob/2016/session%209/images/errorwp.png?raw=true "error message")

### Configure WordPress

Alright, final step. Find wp-config-sample.php in your directory.

[It will look exactly like this.](https://github.com/RavensbourneWebMedia/MagazineMashUp/blob/2016/session%209/wp-config-sample.php "It will look exactly like this.")

Don’t be nervous. Change the database name, username, and password, from this:

```
/** The name of the database for WordPress */
define('DB_NAME', 'database_name_here');
/** MySQL database username */
define('DB_USER', 'username_here');
/** MySQL database password */
define('DB_PASSWORD', 'password_here');


to this:

```
/** The name of the database for WordPress */
define('DB_NAME', 'startwordpress');
/** MySQL database username */
define('DB_USER', 'root');
/** MySQL database password */
define('DB_PASSWORD', 'root');


Find this:

```
$table_prefix  = 'wp_';


And change it to literally anything else with numbers and letters. For security. ‘xyz_’ or “735hjq9_”, etc.

```
$table_prefix  = 'xyz77_';
