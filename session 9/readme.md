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

```
/*
Theme Name: Start WordPress
Author: Tania Rascia
Description: Bootstrap Blog template converted to WordPress
Version: 0.0.1
Tags: bootstrap
*/
```

**index.php**

Remember the Bootstrap blog source code from earlier in the article? Move those two files – `index.html` and `blog.css` – to your custom theme folder. Rename `index.html` to `index.php`.

Your theme has now been created. Go to the WordPress dashboard, and click on `Appearance > Themes`. You’ll see the theme in the collection with all the default themes.

![alt text](https://github.com/RavensbourneWebMedia/MagazineMashUp/blob/2016/session%209/images/startwp.png?raw=true "startwp")





Activate the theme and go back to your main URL. Yep, it’s that simple. You’ve technically created a custom theme already. Of course, it doesn’t do anything yet beyond what a static HTML site can do, but you’re all set up now.

There is one thing you might notice – `blog.css` is not being loaded. Bootstrap’s main CSS and JS files are loading via CDN, but my local css file isn’t loading. Why?

My local URL may be `startwordpress.dev/`, but it’s really pulling from `wp-content/themes/startwordpress`. If I link to `blog.css` with `<link href="blog.css">`, it tries to load `startwordpress.dev/blog.css`, which does not exist.

**Learn right now that you can never link to anything in a WordPress page without some PHP.**

Fortunately, this is easily remedied. There’s a few ways to do this, but I’ll show you the easiest way to start.

Locate where you linked to the CSS stylesheet in the head of `index.php`.

```
<link href="blog.css" rel="stylesheet">
```

We need to tell it to dynamically link to the themes folder. Replace your code with this.

```
<link href="<?php bloginfo('template_directory');?>/blog.css" rel="stylesheet">
```





If you reload the page, you’ll see that CSS is now loading in. The concept will be the same for images, javascript, and most other files you have in the themes folder, except PHP files.

    > Note that this is not the most correct way to load scripts into your site. It’s the easiest to understand and it works, so it’s how we’ll do it for now.

Dividing your page into sections

Right now, everything is in `index.php`. But obviously we want the header, footer and sidebar on all the pages to be the same, right? (Maybe some pages will have slight customization, but that comes later.)

We’re going to divide `index.php` into four sections – `header.php`, `footer.php`, `sidebar.php` and `content.php`.

Here’s the original `index.php`. Now we start cutting and pasting.

**Header – header.php**

Everything from `<!DOCTYPE html>` to the main blog header will be in the header file. The header usually contains all the necessary head styles and the top navigation to the website. The only addition I will make to the code is adding `<?php wp_head(); ?>` right before the closing `</head>`.

```
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="utf-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<meta name="description" content="">
	<meta name="author" content="">

	<title>Blog Template for Bootstrap</title>
	<link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css" rel="stylesheet">
	<link href="<?php bloginfo('template_directory');?>/blog.css" rel="stylesheet">
	<!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
	<!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>
      <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
    <![endif]-->
	<?php wp_head();?>
</head>

<body>

	<div class="blog-masthead">
		<div class="container">
			<nav class="blog-nav">
				<a class="blog-nav-item active" href="#">Home</a>
				<a class="blog-nav-item" href="#">New features</a>
				<a class="blog-nav-item" href="#">Press</a>
				<a class="blog-nav-item" href="#">New hires</a>
				<a class="blog-nav-item" href="#">About</a>
			</nav>
		</div>
	</div>

	<div class="container">

		<div class="blog-header">
			<h1 class="blog-title">The Bootstrap Blog</h1>
			<p class="lead blog-description">The official example template of creating a blog with Bootstrap.</p>
		</div>
    ```
