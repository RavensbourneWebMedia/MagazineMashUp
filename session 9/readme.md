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


## Configure WordPress

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


## Creating your custom theme

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


### Footer – footer.php 

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


## Main Settings

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


## The Loop

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

## Menu and Pages

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


### Individual Post Pages

We have made header, footer, sidebar, content, and page files. Now we’re going to make `single.php`, which is an individual post page. It’s going to be an exact duplicate of `page.php`, except I’m going to change `'content'` to `'content-single'`.

```
<?php get_header(); ?>

	<div class="row">
		<div class="col-sm-12">

			<?php 
				if ( have_posts() ) : while ( have_posts() ) : the_post();
					get_template_part( 'content-single', get_post_format() );
				endwhile; endif; 
			?>

		</div> <!-- /.col -->
	</div> <!-- /.row -->

<?php get_footer(); ?>
```

Now you’ll create `content-single.php`, which is a duplicate of `content.php`.

```
<div class="blog-post">
	<h2 class="blog-post-title"><?php the_title(); ?></h2>
	<p class="blog-post-meta"><?php the_date(); ?> by <a href="#"><?php the_author(); ?></a></p>
 <?php the_content(); ?>
</div><!-- /.blog-post -->
```

So now you can see that `index.php` is pulling in `content.php`, and `single.php` is pulling in `content-single.php`.

Going back to the original `content.php`, we have the title of each article.

```
<h2 class="blog-post-title"><?php the_title(); ?></h2>
```

Using the_permalink(), we’re going to link to the single page.

```
<h2 class="blog-post-title"><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></h2>
```

Now you have a blog posts on the main page that are linking individual blog post page.

Finally, we’ll want to change `the_content()` to `the_excerpt()` on `content.php`. The excerpt will only show the first 55 words of your post, instead of the entire contents.

```
<?php the_excerpt(); ?>
```

### Pagination


In the original Bootstrap blog example, there is pagination to be able to click through multiple pages if you have many blog posts.


Currently, your `index.php` file looks like this.

```
<?php get_header(); ?>

	<div class="row">
		<div class="col-sm-8 blog-main">

			<?php 
			if ( have_posts() ) : while ( have_posts() ) : the_post();
				get_template_part( 'content', get_post_format() );
			endwhile; endif; ?>

		</div>	<!-- /.blog-main -->
		<?php get_sidebar(); ?>
	</div> 	<!-- /.row -->

<?php get_footer(); ?>
```

If you’ll notice, the loop has if and while, then later endif and endwhile. To insert pagination, we’ll have to put it after the endwhile but before the endif. This means that it won’t repeat for each loop, but will only show up once based on posts.

Pagination links are called like this:

```
<?php next_posts_link( 'Older posts' ); ?>
<?php previous_posts_link( 'Newer posts' ); ?>
```

In `index.php`, between `endwhile;` and `endif;`, I’m going to place this code.

```
<nav>
	<ul class="pager">
		<li><?php next_posts_link( 'Previous' ); ?></li>
		<li><?php previous_posts_link( 'Next' ); ?></li>
	</ul>
</nav>
```

By default, 10 posts will show up on a page before it will link to another page. For testing purposes, I’m going to go to Settings > Reading and change Blog pages show at most to 1.


Now we have functioning pagination.



### Comments


One of the biggest advantages WordPress and server based content management systems have over static site generators is the ability to include comments without using a third party. (However, static site generators have many more advantages.

Comments seem complicated to set up, but it doesn’t have to be hard at all. First, we’re going to go back to `single.php` and enable the comments.

Right now, the code looks like this.

```
if ( have_posts() ) : while ( have_posts() ) : the_post();
	get_template_part( 'content-single', get_post_format() );
endwhile; endif; 
We’re going to change it to look like this.

if ( have_posts() ) : while ( have_posts() ) : the_post();
	get_template_part( 'content-single', get_post_format() );

	if ( comments_open() || get_comments_number() ) :
	  comments_template();
	endif;

endwhile; endif; 
```

This is just telling the single post to display the comments template. Now we’ll create `comments.php`.

```
<?php if ( post_password_required() ) {
	return;
} ?>
	<div id="comments" class="comments-area">
		<?php if ( have_comments() ) : ?>
			<h3 class="comments-title">
				<?php
				printf( _nx( 'One comment on “%2$s”', '%1$s comments on “%2$s”', get_comments_number(), 'comments title'),
					number_format_i18n( get_comments_number() ), get_the_title() );
				?>
			</h3>
			<ul class="comment-list">
				<?php 
				wp_list_comments( array(
					'short_ping'  => true,
					'avatar_size' => 50,
				) );
				?>
			</ul>
		<?php endif; ?>
		<?php if ( ! comments_open() && get_comments_number() && post_type_supports( get_post_type(), 'comments' ) ) : ?>
			<p class="no-comments">
				<?php _e( 'Comments are closed.' ); ?>
			</p>
		<?php endif; ?>
		<?php comment_form(); ?>
	</div>
```

Comments are not the simplest part of WordPress theming, but I’ve managed to reduce it down to a small enough code block.

First, we’re setting functionality to prevent users from posting comments if you’ve set your settings to password protected comments `(post_password_required())`. Then we’re creating a comments div, and if there are comments `(have_comments())`, it will display how many comments there are on the post `(get_comments_number())`, followed by the list of comments `(wp_list_comments())`. If the comments are closed `(! comments_open())`, it will let you know; at the end will be the form to submit a comment `(comment_form())`.

Without adding any styles, here is how the functioning single blog post looks.


![alt text](https://github.com/RavensbourneWebMedia/MagazineMashUp/blob/2016/session%209/images/Asset.jpg "comments")

Obviously the styles aren’t quite there yet, but I don’t want to focus on that in this session. Remove the list-style on the uls, add some padding and margins and possibly some borders and background colors, and you’ll have a much prettier comment setup.

Of course, you might want to show how many comments there are or link to the comments from the main page. You can do that with this code inserted into `content.php`.

```
<a href="<?php comments_link(); ?>">
	<?php
	printf( _nx( 'One Comment', '%1$s Comments', get_comments_number(), 'comments title', 'textdomain' ), number_format_i18n( 						get_comments_number() ) ); ?>
</a>
```

Now that we have pagination, blog posts, and comments set up, we can move on to functions.

## Using and Understanding the WordPress Functions File

Located in your theme directory, you can create a file called functions.php. You can use `functions.php` to add functionality and change defaults throughout WordPress. Plugins and custom functions are basically the same – any code you create can be made into a plugin, and vice versa. The only difference is that anything you place in your theme’s functions is only applied while that theme is actively selected.

I have a README on GitHub of useful WordPress functions, which might come in handy the more you use them.
`functions.php` seems complicated, but it’s mostly made up of a bunch of code blocks that, simplified, look like this:

```
function custom_function() {
	//code
}
add_action( 'action', 'custom_function');
```

So, we’re creating our custom function, and adding it in based on action references. Within this file, you can pretty much change or override anything in WordPress.

Let’s go ahead and make `functions.php` and place it in our theme directory.

Since it’s a PHP file, it needs to be begin with the opening PHP tag. It doesn’t need a closing tag; pure PHP files don’t need closing tags.

`<?php`

Eventually, you can insert these types of functions into your own custom plugin that can be used across many themes, but for now we’ll learn how to do it in the theme specific file.
Enqueue Scripts and Stylesheets

By the end of the last article, I was incorrectly linking to my CSS and JavaScript in the header and footer, respectively. This should be done through the functions file.

First, delete the links to the stylesheets and scripts that you have in your header and footer. They’re no longer going to be hard coded into the theme.

I’m going to make css, js and images directories in the root of my theme. So here’s what I have:

* css
* bootstrap.min.css
* style.css
* js
* bootstrap.min.js

Now here’s the first code block we’re going to put in functions.php:

```
// Add scripts and stylesheets
function startwordpress_scripts() {
	wp_enqueue_style( 'bootstrap', get_template_directory_uri() . '/css/bootstrap.min.css', array(), '3.3.6' );
	wp_enqueue_style( 'blog', get_template_directory_uri() . '/css/blog.css' );
	wp_enqueue_script( 'bootstrap', get_template_directory_uri() . '/js/bootstrap.min.js', array('jquery'), '3.3.6', true );
}

add_action( 'wp_enqueue_scripts', 'startwordpress_scripts' );
```

In order for these to properly be inserted into your theme, `<?php wp_head(); ?>` needs to be placed before the closing `</head>` tag, and `<?php wp_footer(); ?>` before the closing `</body>` tag.

By common WordPress convention, I’m naming my script after my theme `(startwordpress_scripts())`. `wp_enqueue_style` is for inserting CSS, and `wp_enqueue_script` for JS. After that, the array contains the ID, location of the file, an additional array with required depenedencies `(such as jQuery)`, and the version number.

Now we have jQuery, Bootstrap CSS, Bootstrap JS, and custom CSS being properly loaded into the website.

## Enqueue Google Fonts

The function to include the Google Fonts stylesheets is slightly different, based on the dynamic nature of the URL. Here is an example using Open Sans.

```
// Add Google Fonts
function startwordpress_google_fonts() {
				wp_register_style('OpenSans', 'http://fonts.googleapis.com/css?family=Open+Sans:400,600,700,800');
				wp_enqueue_style( 'OpenSans');
		}


add_action('wp_print_styles', 'startwordpress_google_fonts');
```

Now I have Open Sans by Google Fonts linked in my page.

# Fix the WordPress Title

If you’ll notice, we’re currently pulling in the title for the website with this code.

```
<title><?php echo get_bloginfo( 'name' ); ?></title>
```

This is not very intuitive – it means that whatever you have set as your website’s title will be the title tag for every page. However, we’re going to want each individual page to show the title of the article first, and also include a reference to the main site title.

Introduced in WordPress 4.1 is the ability to simply have WordPress take care of the title tag in an intuitive way. Simply remove the title tag from your header.php entirely, and in `functions.php`, add this code block.

```
// WordPress Titles
add_theme_support( 'title-tag' );
```

## Create Global Custom Fields

Sometimes, you might have custom settings that you want to be able to set globally. An easy example on this page is the social media links on the sidebar.


Right now these links aren’t leading anywhere, but we want to be able to edit it through the admin panel. The source of this code is modified from this Settings API tutorial.

First, we’re going to add a section on the left hand menu called Custom Settings.


```
// Custom settings
function custom_settings_add_menu() {
  add_menu_page( 'Custom Settings', 'Custom Settings', 'manage_options', 'custom-settings', 'custom_settings_page', null, 99);
}
add_action( 'admin_menu', 'custom_settings_add_menu' );
```

Then we’re going to create a basic page.


```
// Create Custom Global Settings
function custom_settings_page() { ?>
  <div class="wrap">
    <h1>Custom Settings</h1>
    <form method="post" action="options.php">
       <?php
           settings_fields('section');
           do_settings_sections('theme-options');      
           submit_button(); 
       ?>          
    </form>
  </div>
<?php }
```

The code contains a form posting to `options.php`, a section and theme-options, and a submit button.

Now we’re going to create an input field for Twitter.

```
// Twitter
function setting_twitter() { ?>
  <input type="text" name="twitter" id="twitter" value="<?php echo get_option('twitter'); ?>" />
<?php }
```

Finally, we’re going to set up the page to show, accept and save the option fields.

```
function custom_settings_page_setup() {
  add_settings_section('section', 'All Settings', null, 'theme-options');
  add_settings_field('twitter', 'Twitter URL', 'setting_twitter', 'theme-options', 'section');

  register_setting('section', 'twitter');
}
add_action( 'admin_init', 'custom_settings_page_setup' );
```

Now I’ve saved my Twitter URL in the field.


For good measure, I’m going to add another example, this time for GitHub.

```
function setting_github() { ?>
  <input type="text" name="github" id="github" value="<?php echo get_option('github'); ?>" />
<?php }
Now you’ll just duplicate the fields in custom_settings_page_setup.

  add_settings_field('twitter', 'Twitter URL', 'setting_twitter', 'theme-options', 'section');
  add_settings_field('github', 'GitHub URL', 'setting_github', 'theme-options', 'section');
  
	register_setting('section', 'twitter');
  register_setting('section', 'github');
```

Now back in `sidebar.php`, I’m going to change the links from this:

```
<li><a href="#">GitHub</a></li>
<li><a href="#">Twitter</a></li>
```

To this:

```
<li><a href="<?php echo get_option('github'); ?>">GitHub</a></li>
<li><a href="<?php echo get_option('twitter'); ?>">Twitter</a></li>
```

And now the URLs are being dynamically generated from the custom settings panel!

## Featured Image


You might want to have a featured image for each blog post. This functionality is not built into the WordPress core, but is extremely easy to implement. Place this code in your functions.php.

```
// Support Featured Images
add_theme_support( 'post-thumbnails' );
```

Now you’ll see an area where you can upload an image on each blog post.

I’m just going to upload something I drew in there for an example. Now, display the image in `content-single.php`.

```
<?php if ( has_post_thumbnail() ) {
  the_post_thumbnail();
} ?>
```

Now you have an image on your individual post pages! If you wanted the thumbnail to show up on on the main blog page as well, you could do something like this on `content.php` to split the page if a thumbnail is present:

```
<?php if ( has_post_thumbnail() ) {?>
	<div class="row">
		<div class="col-md-4">
			<?php	the_post_thumbnail('thumbnail'); ?>
		</div>
		<div class="col-md-6">
			<?php the_excerpt(); ?>
		</div>
	</div>
	<?php } else { ?>
	<?php the_excerpt(); ?>
	<?php } ?>
```

## Custom Post Types

One of the most versatile way to extend your WordPress site as a full blown content management system is with custom post types. A custom post type is the same as Posts, except you can add as many of them as you want, and with as much custom functionality as you want.

If you’re interested in using plugins, you can download the Advanced Custom Fields plugin, which will add a great deal of customizability to your theme with little effort.

For now, I’m going to show you how to set up a simple custom post type, and call the post in it’s own loop. There is much more that can be done with custom post types, but that’s a bit more complicated and deserves an article all of its own.

Custom Post Types on the WordPress codex will also give you more insight on some of the possibilities available.

In `functions.php`, I’m going to create the custom post type called My Custom Post.


```
// Custom Post Type
function create_my_custom_post() {
	register_post_type('my-custom-post',
			array(
			'labels' => array(
					'name' => __('My Custom Post'),
					'singular_name' => __('My Custom Post'),
			),
			'public' => true,
			'has_archive' => true,
			'supports' => array(
					'title',
					'editor',
					'thumbnail',
				  'custom-fields'
			)
	));
}
add_action('init', 'create_my_custom_post');
```

In the `create_my_custom_post()`, I’ve created a post called My Custom Post with a slug of my-custom-post. If my original URL was example.com, the custom post type would appear at example.com/my-custom-post.

In supports, you can see what I’m adding – title, editor, thumbnail, and custom fields. These translate to the fields on the back end that will be available.

* title is the title field that I call with `<?php the_title(); ?>`.
* editor is the content editing area that I call with `<?php the_content(); ?>`.
* thumbnail is the featured image that I call with `<?php the_post_thumbnail(); ?>`.
* custom-fields are custom fields that I can add in and call later.
*I’ve decided I’m going to make a new page for the custom post to loop in. I created a page called Custom, which will appear at `example.com/custom`. Right now, my page is pulling from `page.php`, like all the other pages.

I’m going to create `page-custom.php`, and copy the code over from `page.php`. According to the WordPress template hierarchy, a `page-name.php` will override `page.php`.


**The original loop we used looked like this:**

```
if ( have_posts() ) : while ( have_posts() ) : the_post();
	// Contents of the Loop
endwhile; endif; 
```

A custom post type loop will look like this:

```
$custom_query = new WP_Query( $args );
while ($custom_query->have_posts()) : $custom_query->the_post();
  // Contents of the custom Loop
endwhile;
```


Note that this only a while, and does not have an if or endif.

I’ll have to define the $args or arguments, before the loop.


```
$args =  array( 
	'post_type' => 'my-custom-post',
	'orderby' => 'menu_order',
	'order' => 'ASC'
);
```

Here I’m defining the post type as my-custom-post, and ordering the posts in ascending order.

So here’s the entire code for `page-custom.php`.

```
<?php get_header(); ?>

	<div class="row">
		<div class="col-sm-12">

			<?php 
				$args =  array( 
					'post_type' => 'my-custom-post',
					'orderby' => 'menu_order',
					'order' => 'ASC'
				);
				 $custom_query = new WP_Query( $args );
            while ($custom_query->have_posts()) : $custom_query->the_post(); ?>

				<div class="blog-post">
					<h2 class="blog-post-title"><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></h2>
					<?php the_excerpt(); ?>
				</div>

				<?php endwhile; ?>
		</div> <!-- /.col -->
	</div> <!-- /.row -->

	<?php get_footer(); ?>
```

Now `example.com/custom` will only pull in posts from the custom post type we created. Right now, the custom post type is set up to only do things that the normal posts can do, but the more you fall down the rabbit hole, the more possibilities you discover.




##Create a Custom Post

I’m starting off with a completely empty WordPress theme, just like in part one. I’m going to create a custom post called Your Post, with the id your_post. You can call it whatever you want, but it might be easiest to practice the first time around with the same names I used.

Here’s the code that will go into `functions.php`.

```
<?php

function create_post_your_post() {
	register_post_type( 'your_post',
		array(
			'labels'       => array(
				'name'       => __( 'Your Post' ),
			),
			'public'       => true,
			'hierarchical' => true,
			'has_archive'  => true,
			'supports'     => array(
				'title',
				'editor',
				'excerpt',
				'thumbnail',
			), 
			'taxonomies'   => array(
				'post_tag',
				'category',
			)
		)
	);
	register_taxonomy_for_object_type( 'category', 'your_post' );
	register_taxonomy_for_object_type( 'post_tag', 'your_post' );
}
add_action( 'init', 'create_post_your_post' );
```

You can go more in-depth about all the options of creating a custom post here. I made a very simple one. The most important things that it does:


* Registers a post type called Your Post, with the id your_post.
* Supports a title – `the_title()`, editor – `the_content()`, excerpt – `the_excerpt()`, and thumbnail/featured image – `the_post_thumbnail()`. (Must add theme support for the thumbnail).
* Supports taxonomies, or ways to group posts, with tags and categories.
Displaying the Custom Post
* Now, to display the post on the front end. You can display the contents of a custom post anywhere. I’m just going to put it in page.php for testing purposes, so any page I make will display my custom loop.

```
<?php 

$args = array(
	'post_type' => 'your_post',
);  
$your_loop = new WP_Query( $args ); 

if ( $your_loop->have_posts() ) : while ( $your_loop->have_posts() ) : $your_loop->the_post(); 
$meta = get_post_meta( $post->ID, 'your_fields', true ); ?>

<!-- contents of Your Post -->

<?php endwhile; endif; wp_reset_postdata(); ?>
```

The variable `$your_loop` can be anything, I’m just sticking to a common theme of your_ throughout this article so you know what you can change. `$meta = get_post_meta( $post->ID`, `'your_fields', true );` is not necessary right now, but will be essential later.


## Back End
Updating the title and editor fields like in a regular post. 


##Code
Inserting the template codes like normal.

```
<h1>Title</h1>
<?php the_title(); ?>

<h1>Content</h1>
<?php the_content(); ?>
```

## Front End
And here we are. I have no styles or anything, because it’s not necessary for the point of the article, and I don’t like adding unnecessary complexity.


Alright, so the custom post is all set up 

## Create a Meta Box

From here, the original source for much of the code comes from WordPress Meta Boxes: a Comprehensive Developer’s Guide by Alex Mansfield, and Create WordPress Post Custom Meta Boxes by Paulund. Both of those are very good, detailed articles, and I’ve attempted to simplify some of those processes here.

Here’s the code to add a meta box.

```
function add_your_fields_meta_box() {
	add_meta_box(
		'your_fields_meta_box', // $id
		'Your Fields', // $title
		'show_your_fields_meta_box', // $callback
		'your_post', // $screen
		'normal', // $context
		'high' // $priority
	);
}
add_action( 'add_meta_boxes', 'add_your_fields_meta_box' );
```

More detail on this function can be found here. The actual function here is `add_meta_box( $id, $title, $callback, $screen, $context, $priority )`. The most important ones are `$title`, which is Your Fields, and `$screen` (or page), which is where the meta box will be added. You can add it to a regular post or page, among other things, but I chose to add it to your_post, our custom post from earlier.

Now if you go back into your post, you’ll see this below the editor.

**PICTURES**

It’s an empty meta box! Additionally, if you click on Screen Options at the top of the post, you’ll see Your Fields in the options.

**PIC**

Neat. Now it’s time to start putting stuff in there.

## Save Fields in the Database

First, the function that will display all your custom fields.

```
function show_your_fields_meta_box() {
	global $post;  
		$meta = get_post_meta( $post->ID, 'your_fields', true ); ?>

	<input type="hidden" name="your_meta_box_nonce" value="<?php echo wp_create_nonce( basename(__FILE__) ); ?>">

    <!-- All fields will go here -->

	<?php }
```

We’ll return to <!-- All fields will go here --> in a moment. For now, directly below the above function, we’re going to paste in this big chunk of code (modified from Paulund) that will save all your_fields to the database.

```
function save_your_fields_meta( $post_id ) {   
	// verify nonce
	if ( !wp_verify_nonce( $_POST['your_meta_box_nonce'], basename(__FILE__) ) ) {
		return $post_id; 
	}
	// check autosave
	if ( defined( 'DOING_AUTOSAVE' ) && DOING_AUTOSAVE ) {
		return $post_id;
	}
	// check permissions
	if ( 'page' === $_POST['post_type'] ) {
		if ( !current_user_can( 'edit_page', $post_id ) ) {
			return $post_id;
		} elseif ( !current_user_can( 'edit_post', $post_id ) ) {
			return $post_id;
		}  
	}
	
	$old = get_post_meta( $post_id, 'your_fields', true );
	$new = $_POST['your_fields'];

	if ( $new && $new !== $old ) {
		update_post_meta( $post_id, 'your_fields', $new );
	} elseif ( '' === $new && $old ) {
		delete_post_meta( $post_id, 'your_fields', $old );
	}
}
add_action( 'save_post', 'save_your_fields_meta' );
```

Make sure your_meta_box_nonce matches the name attribute, and you’ve specified your_fields in the meta box function. This code is verifying the Nonce from the first function, making sure the user has the correct permissions to update the fields, and updating the post meta fields.

It’s a confusing block of code at first, but fortunately you don’t have to do much with this one besides make sure the id of the post matches the custom meta box you created.

## Create Custom Fields
Now we’re going to return to <!-- All fields will go here -->. This is where we’re going to make our input text field, textbox, checkbox, select menu, and image upload. Right above that, we created a variable called `$meta`. This reaches into the your_fields table in the database and retrieves the information: `$meta = get_post_meta( $post->ID, 'your_fields', true );`. We’re going to make an array and put all of our custom fields in it.

You can use custom CSS to make your admin panel look nice, which I can possibly go into further if requested, but getting the basic functionality is more important to me for now, so I’m just going to use the default styles. Wrapping everything in a <p> isn’t semantically correct, but it’s keeping it organized for now.

## Text Input

I’m going to add a regular text input. The regular-text class is just a built in WordPress admin style. Whatever you put in the straight brackets will be the code for your custom field. If I wanted to make this text field an e-mail address, for example on a custom post called “Team Members” with contact information for each team member, I might call it your_fields[email] instead of your_fields[text].

```
<p>
	<label for="your_fields[text]">Input Text</label>
	<br>
	<input type="text" name="your_fields[text]" id="your_fields[text]" class="regular-text" value="<?php echo $meta['text']; ?>">
</p>
```

## Textarea

The code for the text area is almost the same as the input, except the value is echoed out between the tags instead of as an attribute. The rows, cols, and style placed on it doesn’t really matter. It’s important to leave no space between the tags, to ensure no extra space ends up in your textbox.

```
<p>
	<label for="your_fields[textarea]">Textarea</label>
	<br>
	<textarea name="your_fields[textarea]" id="your_fields[textarea]" rows="5" cols="30" style="width:500px;"><?php echo $meta['textarea']; ?></textarea>
</p>
```

## Checkbox

There might be several ways to implement the checkbox, but this is one way that worked for me.

```
<p>
	<label for="your_fields[checkbox]">Checkbox
		<input type="checkbox" name="your_fields[checkbox]" value="checkbox" <?php if ( $meta['checkbox'] === 'checkbox' ) echo 'checked'; ?>>
	</label>
</p>
```

## Select Menu

You can include as many options as you want here, but I’m just doing two for the example.

```
<p>
	<label for="your_fields[select]">Select Menu</label>
	<br>
	<select name="your_fields[select]" id="your_fields[select]">
			<option value="option-one" <?php selected( $meta['select'], 'option-one' ); ?>>Option One</option>
			<option value="option-two" <?php selected( $meta['select'], 'option-two' ); ?>>Option Two</option>
	</select>
</p>
```

## Image
The image upload is going to be the most complicated one. The actual display code isn’t any more complicated than the other fields have been. It’s important to include the meta-image class on the text input, and image-upload class on the submit button. I just put some quick max-width styling on the image for a simple preview.


```
<p>
	<label for="your_fields[image]">Image Upload</label><br>
	<input type="text" name="your_fields[image]" id="your_fields[image]" class="meta-image regular-text" value="<?php echo $meta['image']; ?>">
	<input type="button" class="button image-upload" value="Browse">
</p>
<div class="image-preview"><img src="<?php echo $meta['image']; ?>" style="max-width: 250px;"></div>
```

Actually getting this to do anything is the more complicated part. Directly below, you can place this JavaScript snippet (modified from Theme Foundation).

This code will open the built in WordPress media gallery when you click browse, and insert the selected URL into the input field.

```
<script>
jQuery(document).ready(function ($) {

	// Instantiates the variable that holds the media library frame.
	var meta_image_frame;
	// Runs when the image button is clicked.
	$('.image-upload').click(function (e) {
		e.preventDefault();
		var meta_image = $(this).parent().children('.meta-image');

		// If the frame already exists, re-open it.
		if (meta_image_frame) {
			meta_image_frame.open();
			return;
		}
		// Sets up the media library frame
		meta_image_frame = wp.media.frames.meta_image_frame = wp.media({
			title: meta_image.title,
			button: {
				text: meta_image.button
			}
		});
		// Runs when an image is selected.
		meta_image_frame.on('select', function () {
			// Grabs the attachment selection and creates a JSON representation of the model.
			var media_attachment = meta_image_frame.state().get('selection').first().toJSON();
			// Sends the attachment URL to our custom image input field.
			meta_image.val(media_attachment.url);
		});
		// Opens the media library frame.
		meta_image_frame.open();
	});
});
</script>
```

And that’s everything that needs to go into the `functions.php` file! As with the CSS styling from earlier, we can place this JavaScript in a separate file and call it on the admin pages, but for simplicity I’m just including it in the `functions.php` file for now. At the end of the article, I’ll include the entire code in case you got lost anywhere along the way.

## Display the Output

Now, we have all our fields, and they all appear in the Your Fields meta box. I’m going to fill them all out, check the checkbox, select an option, upload an image of the majestic Doge Lion, and publish the post.

These will all go in the `$your_loop query`. Make sure `$meta = get_post_meta( $post->ID, 'your_fields', true );` is in your loop.

**PICTURE**

## Text Input

Simple output of the text field.

```
<h1>Text Input</h1>
<?php echo $meta['text']; ?>

## Textarea
Simple output of the textarea.

```
<h1>Textarea</h1>
<?php echo $meta['textarea']; ?>
```

## Checkbox
I’m going to check if the checkbox has been checked or not, and display a message based on it.

```
<h1>Checkbox</h1>
<?php if ( $meta['checkbox'] === 'checkbox') { ?>
Checkbox is checked.
<?php } else { ?> 
Checkbox is not checked. 
<?php } ?>
```

## Select
I’ll show you two things you can do with the select. The basic output will be the value attribute.

```
<h1>Select Menu</h1>
<p>The actual value selected.</p>
<?php echo $meta['select']; ?>
```

You can also use a PHP switch statement to display a specific message based on the value selected.

```
<p>Switch statement for options.</p>
<?php 
	switch ( $meta['select'] ) {
		case 'option-one':
			echo 'Option One';
			break;
		case 'option-two':
			echo 'Option Two';
			break;
		default:
			echo 'No option selected';
			break;
	} 
?>
````

## Image
And finally, displaying the image.

```
<h1>Image</h1>
<img src="<?php echo $meta['image']; ?>">
```

Here is the final output of everything I filled out earlier.

**PCITURE**





