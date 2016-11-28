# Starting a Wordpress blog from scratch

### Install MAMP

You can find [MAMP](https://www.mamp.info/en/downloads/) here.

What is MAMP?

It is local host on your computer (Mac of course).

### Download Bootstrap blog theme

This is one of the default themes on [Bootstrap’s official website.](http://getbootstrap.com/examples/blog/ "Bootstrap’s official website")

I have nicely set one up on [git for you](https://github.com/RavensbourneWebMedia/MagazineMashUp/tree/2016/session%209/bootstrapblog "gitgitbootstrap")
 already.




### Installing 

**Create a place for WordPress to live**

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



### style.css

In your custom theme folder, create style.css. It simply contains a comment that alerts WordPress that a theme exists here. Change the name, author, description, and so on.

```
/*
Theme Name: Start WordPress
Author: Tor Njamo
Description: Bootstrap Blog template converted to WordPress
Version: 0.0.1
Tags: bootstrap
*/
```


### index.php

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



### Header – header.php

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



###Footer – footer.php 

Same deal for the footer as the header. It will include whatever visible footer you have, your JS links (for now) and `<?php wp_footer(); ?>` right before `</body>`. Since I included the .container div in the header, I’m going to close it in the footer.


```
</div> <!-- /.container -->

		<footer class="blog-footer">
      <p>Blog template built for <a href="http://getbootstrap.com">Bootstrap</a> by <a href="https://twitter.com/mdo">@mdo</a>.</p>
      <p>
        <a href="#">Back to top</a>
      </p>
    </footer>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/js/bootstrap.min.js"></script>
<?php wp_footer(); ?>
  </body>
</html>
```



### Sidebar – sidebar.php 

Most websites, especially blogs, will have a side area for including content such as archives, tags, categories, ads, and whatnot. (Content removed for brevity.)

```
<div class="col-sm-3 col-sm-offset-1 blog-sidebar">
	<div class="sidebar-module sidebar-module-inset">
		<h4>About</h4>
		<p>Etiam porta <em>sem malesuada magna</em> mollis euismod. Cras mattis consectetur purus sit amet fermentum. Aenean lacinia bibendum nulla sed consectetur.</p>
	</div>
	<div class="sidebar-module">
		<h4>Archives</h4>
		<ol class="list-unstyled">
			<li><a href="#">March 2014</a></li>
			<!-- More archive examples -->
		</ol>
	</div>
	<div class="sidebar-module">
		<h4>Elsewhere</h4>
		<ol class="list-unstyled">
			<li><a href="#">GitHub</a></li>
			<li><a href="#">Twitter</a></li>
			<li><a href="#">Facebook</a></li>
		</ol>
	</div>
</div><!-- /.blog-sidebar -->
```



### Content – content.php

If the sidebar is where all the secondary information goes, the content is where all the articles and main content of the website go. (Content removed for brevity.)

```
<div class="blog-post">
	<h2 class="blog-post-title">Sample blog post</h2>
	<p class="blog-post-meta">January 1, 2014 by <a href="#">Mark</a></p>

	<p>This blog post shows a few different types of content that's supported and styled with Bootstrap. Basic typography, images, and code are all supported.</p>
	<hr>

<!-- the rest of the content -->

</div><!-- /.blog-post -->
```



### Index

The index file should be pretty sparse now. In fact, it should only be this:

```
<div class="row">
	<div class="col-sm-8 blog-main">
	</div> <!-- /.blog-main -->
</div> 	<!-- /.row -->
```

Now we’re going to add everything back in. Here’s your new `index.php`.

```
<?php get_header(); ?>

	<div class="row">

		<div class="col-sm-8 blog-main">

			<?php get_template_part( 'content', get_post_format() ); ?>

		</div> <!-- /.blog-main -->

		<?php get_sidebar(); ?>

	</div> <!-- /.row -->

<?php get_footer(); ?>
```

Even if you’ve never used PHP before, this code is all very self explanatory. `get_header();`, `get_sidebar();` and `get_footer();` are all functions that look for their respective .php files and insert the code. Of course, they all go inside their own `<?php ?>` tags to let the server know to parse them as HTML. The content function is slightly different, but it does the same thing.

If you re-load your URL, your entire site is now loaded, just as before. You will notice a top bar if you’re logged in to the back end.




### Main Settings

Before we start pulling in posts and pages, we need to configure some main settings of WordPress. For example, my title right now is `“The Bootstrap Blog”`, hard coded in HTML. I want the `<title>` and `h1` of my site to be changeable through the back end.

In your dashboard, go to Settings > General. Set your title.

![alt text](https://github.com/RavensbourneWebMedia/MagazineMashUp/blob/2016/session%209/images/starttheme.png?raw=true "starttheme")

In header.php, change the contents of the title tag and main h1 tag to this code:

```
<?php echo get_bloginfo( 'name' ); ?>
```

And the description to this one.

```
<?php echo get_bloginfo( 'description' ); ?>
```

Finally, I want the title to always take me back to the main blog page. bloginfo('wpurl'); is the code that will do that.

```
<a href="<?php bloginfo( 'wpurl' );?>"><!-- site title --></a>
```

Here’s the full code in case you’re confused.

```
<div class="blog-header">
	<h1 class="blog-title"><a href="<?php bloginfo( 'wpurl' );?>"><?php echo get_bloginfo( 'name' ); ?></a></h1>
	<p class="lead blog-description"><?php echo get_bloginfo( 'description' ); ?></p>
</div>
```

We’ve finally made the first dynamic change to the page. The front end should reflect what you put in your settings.

![alt text](https://github.com/RavensbourneWebMedia/MagazineMashUp/blob/2016/session%209/images/thisisthestart.png?raw=true "wpstart")

Now go to Settings > Permalinks. By default, WordPress is set to Day and name, which is a really ugly URL structure. Click on Post name and apply the changes.




### The Loop

The most exciting part is being able to dynamically insert content, and in WordPress we do that with The Loop. It’s the most important function of WordPress. All of your content is generated through a loop.

In the dashboard, if you click on Posts, you will see a `“Hello, world!”` post in there by default. Our goal is to display that post in the blog.

The Loop itself is quite simple.

```
<?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>

<!-- contents of the loop -->

<?php endwhile; endif; ?>
```

It explains itself – IF there are posts, WHILE there are posts, DISPLAY the post. Anything inside the loop will be repeated. For a blog, this will be the post title, the date, the content, and comments. Where each individual post should end is where the loop will end. We’re going to add the loop to index.php.

Here’s your new index file.

```
<?php get_header(); ?>

	<div class="row">

		<div class="col-sm-8 blog-main">

			<?php
			if ( have_posts() ) : while ( have_posts() ) : the_post();

				get_template_part( 'content', get_post_format() );

			endwhile; endif;
			?>

		</div> <!-- /.blog-main -->

		<?php get_sidebar(); ?>

	</div> <!-- /.row -->

<?php get_footer(); ?>
```

The only thing inside your loop is `content.php`, which will contain the contents of one single post. So open `content.php` and change the contents to this:

```
<div class="blog-post">
	<h2 class="blog-post-title"><?php the_title(); ?></h2>
	<p class="blog-post-meta"><?php the_date(); ?> by <a href="#"><?php the_author(); ?></a></p>

 <?php the_content(); ?>

</div><!-- /.blog-post -->
```

It’s amazingly simple! `the_title();` is the title of the blog post, `the_date();` shows the date, `the_author(); the author`, and `the_content();` is your post content. I added another post to prove at the loop is working.

![alt text](https://github.com/RavensbourneWebMedia/MagazineMashUp/blob/2016/session%209/images/addanotherpost.png?raw=true "addpost")

Awesome. Let’s make the sidebar dynamic, as well. There should be a description and archive list in the sidebar. In the dashboard, I’m going to edit my user description to say “Front end web developer and professional nerd.”

Delete all the lis under Archives and change it to this code.

```
<h4>Archives</h4>
<ol class="list-unstyled">
	<?php wp_get_archives( 'type=monthly' ); ?>
</ol>
```

For my description, I’m going to pull in metadata from my user account.

```
<h4>About</h4>
<p><?php the_author_meta( 'description' ); ?> </p>
```

Here’s the blog so far.


![alt text](https://github.com/RavensbourneWebMedia/MagazineMashUp/blob/2016/session%209/images/theblogsofar.png?raw=true "sofar")

### Menu and Pages

Okay. Now we know how to make a blog, and edit some sidebar content. Only one main aspect of this page remains – the navigation, and where it leads. Well, there are two main aspects to WordPress – Posts and Pages. They’re very similar in that they both use the Loop. However, pages are where you put content that isn’t a blog post. This is where the CMS aspect of WordPress comes in – each individual page can be as customized as you want.

In the dashboard, I added a page so we can see two. First, we’re going to edit the navbar so that the links lead to the pages. Back in header.php, find and change this code.

```
<div class="blog-masthead">
	<div class="container">
		<nav class="blog-nav">
			<a class="blog-nav-item active" href="#">Home</a>
			<?php wp_list_pages( '&title_li=' ); ?>
		</nav>
	</div>
</div>
```

`wp_list_pages();` will list all the pages you have in an unordered list. `'title_li='` is telling the code not to add a “Pages” title before the list. Unfortunately for us, this looks terrible; the original `blog.css` has the links coded in `a` tags, not `li` tags.

![alt text](https://github.com/RavensbourneWebMedia/MagazineMashUp/blob/2016/session%209/images/list1.png?raw=true "tags")

Fortunately, this is a very easy fix. I’m just going to apply the style from one to the other. Add this to blog.css

```
.blog-nav li {
    position: relative;
    display: inline-block;
    padding: 10px;
    font-weight: 500;
}
.blog-nav li a {
    color: #fff;
}
```

![alt text](https://github.com/RavensbourneWebMedia/MagazineMashUp/blob/2016/session%209/images/list2.png?raw=true "muchvetter")

Much better.



### Page
– page.php

I want the pages to have a different layout than the blog posts; I don’t want sidebars on them. Think of index.php as the blog-index and page.php as the page-index. I’m going to create page.php, which will be very similar to the index except have a full 12-wide grid instead of an 8-wide content and 4-wide sidebar.

```
<?php get_header(); ?>

	<div class="row">
		<div class="col-sm-12">

			<?php
				if ( have_posts() ) : while ( have_posts() ) : the_post();

					get_template_part( 'content', get_post_format() );

				endwhile; endif;
			?>

		</div> <!-- /.col -->
	</div> <!-- /.row -->

<?php get_footer(); ?>
```

When I click on my sample page, the layout is now different than the blog post layout.

![alt text](https://github.com/RavensbourneWebMedia/MagazineMashUp/blob/2016/session%209/images/page.png?raw=true "page")


