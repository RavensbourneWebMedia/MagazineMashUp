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
```

to this:

```
/** The name of the database for WordPress */
define('DB_NAME', 'startwordpress');
/** MySQL database username */
define('DB_USER', 'root');
/** MySQL database password */
define('DB_PASSWORD', 'root');
```

Find this:

```
$table_prefix  = 'wp_';
```

And change it to literally anything else with numbers and letters. For security. ‘xyz_’ or “735hjq9_”, etc.

```
$table_prefix  = 'xyz77_';
```

Go to https://api.wordpress.org/secret-key/1.1/salt and replace the entire ‘put your unique phrase here’ with that generated code.

Save the file as `wp-config.php` in your directory.

Now, when you go back to your website and refresh, you should see this screen.

![alt text](https://github.com/RavensbourneWebMedia/MagazineMashUp/blob/2016/session%209/images/language.png?raw=true "language")


You’ll have to input a few things – username, password, e-mail address, and then you’re done. Congratulations, you have successfully installed WordPress! You will be redirected to `/wp-login.php`, where you can input your credentials to log into the backend. If you go to your main URL, You will see the default WordPress blog and “Hello, World!” post.


### Creating your custom theme

Outside of configuring WordPress, almost everything you do in WordPress will be in the `wp-content` folder; everything else is core code, and you don’t want to mess with that.


From this point on, the WordPress Codex and StackOverflow will become your best friends. I’ll show you how to build a basic theme, but how you choose to customize your themes beyond that is totally up to you.


In Finder, follow the path of `wp-content > themes` to arrive at your themes folder. You’ll see the WordPress default themes – `twentyfifteen`, `twentyfourteen`, `twentythirteen` – and `index.php`. Create a new directory for your theme; I called mine startwordpress.


> A WordPress theme needs only two files to exist – `style.css` and `index.php`.



**style.css**

In your custom theme folder, create style.css. It simply contains a comment that alerts WordPress that a theme exists here. Change the name, author, description, and so on.
