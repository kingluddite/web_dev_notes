# WordPress Notes

* [part 1](bootstrap-wordpress-part-1.md)
* [part 2](bootstrap-wordpress-part-2.md)
* [part 3](bootstrap-wordpress-part-3.md)
* [part 4](bootstrap-wordpress-part-4.md)
* [part 5](bootstrap-wordpress-part-5.md)

# WordPress - Building a Custom Theme

## Two ways to install WordPress
1. The traditional (long way)
2. The WP-CLI (short way)

### Let's show you the long way for thoroughness

## [The Traditional Way to Install WordPress](../how-to-install-wordpress.md)
* We assume you are using MAMP
* Other Dev Server Setups
    - XAMPP
    - WAMP
    - Desktop Server
    - VVV, Virtual Box and Vagrant
    - Sage, Trellis, Bedrock

## The Sites folder
* Where do we put all our WordPress sites?
* Not required but used a lot on Mac WP development teams

`$ mkdir ~/Sites`

* We will create the `Sites` folder in our home directory * This is not mandatory but it will help if we all put our sites in the same location
* `Sites` was the traditional folder for older Mac OS systems but they removed it in recent OS systems

## Best Practice
* I never name folders with a capital letter but this is an exception to that rule
* I make it to follow how the folder was traditionally named

## Server Config
* Remember when using MAMP you need to alter your configuration by changing the Server Root to point to `Sites`
* You also can change the ports so you won't have to add the default `:8888` port after your URLS
* Read my MAMP notes to learn how to do this (seach my notes (should be downloaded to your computer) for `mamp.md`)

# WP-CLI
* **note name** `how-to-install-wp-cli.md`
* **tip** Remember searching for files in Sublime text is easy with `ctrl` + `p` and type `how-to-install` and select the file from the dropdown

## Install WordPress the fast way using WP-CLI
* After you install WordPress more than 10 times you will get tired of installing it the long way
* WP-CLI is a command line interface for making installing WordPress less painful

## Example of wp-cli WordPress install
1. Start from scratch
2. Create a folder called `coca-cola-wp` in Sites
3. CD into that folder
4. Create a new database inside **phpMyAdmin** called `coca_cola_wp`
5. While inside `coca-cola-wp` use **wp-cli** with:

`$ wp core download`

  * Downloads WordPress core files

6. `$ wp core config --dbuser=root --dbpass=root --dbname=coca_cola_wp`

  * creates `wp-config.php` with DB connection

7. `$ wp core install --url=http://localhost/coca-cola-wp --title=CocaCola --admin_user=admin --admin_password=password --admin_email=myemail@gmail.com`

  * Creates all the WordPress tables inside your DB and creates the super admin user

8. Visit `http://localhost/coca-cola-wp`

  * You should see your WordPress site

9. Log into WordPress site with `localhost/coca-coloa-wp/wp-admin` and use username: **admin** and password: **password**

## Troubleshoot MAMP/WP-CLI install problems
* `troubleshoot-mamp-wpcli-install-problems.md`

### Where is our WordPress custom theme located?
* In the themes folder `wp-content/themes`

#### So create your custom theme inside this `themes` folder.
* Name it `thunder-tube-theme`.

`$ mkdir wp-content/themes/thunder-tube-theme`

#### Change into that directory

`$ cd wp-content/themems/thunder-tube-theme`

* This folder is where all your custom theme files goes.

## Which version of Bootstrap will we use?
* For this project we will use Bootstrap 4
* A popular CSS framework
* The great thing is it gives you responsiveness and grid layouts right out of the box

## Final Project
* For our final project, you have the choice of using Bootstrap 4 or using your own code

#### Where can I get bootstrap 4?
[Bootstrap 4](https://v4-alpha.getbootstrap.com/)

### Install Node
* You will need to install node

### Why do we need Node?
* The reason is to save time
* In the old days people would grab resources they need for a site (ie Bootstrap)
* They would manually download it and then add the necessary `script` and `link` HTML tags to include those resources
* Node and more specifically `npm` will help us speed up this process
* But before we can use node we must install it.

## Follow these instructions to install node
* how-to-install-node.md

## Version Control
* Coding without a net is scary
* What if you lose something you were working days, weeks or months? That would not be good
* Git and Github is a way to make sure you don't lose your work
* Check the link below to get up and running with Git

## How to Use Git with WordPress
* how-to-use-git-with-wordpress.md
* A large percentage of modern web development jobs are using Git and Github

### Final Project
* You will need to use Git and Github for your final project
* You need to create a Github rebo and push your final project to it
* Email me your github final project repo URL

### Grabbing Bootstrap with npm

#### You need to create `package.json` using npm

##### How to create package.json
* how-to-create-package-json.md

## Add these files to your custom theme folder
* Custom themes can have lots of files
* The required files are `index.php` and `style.css`.
  - functions.php
  - header.php
  - footer.php
  - index.php (required)
  - style.css (required)

### Let's add a background color to our site.

`style.css`

```css
body {
  background-color: red;
}
```

## screenshot.png
* Each WordPress theme has a special image file named `screenshot.png`
* The recommended dimensions are `880 x 660px` and place that file in the root of your custom theme

## Add images with Terminal
* In the root of every WordPress custom theme you need an image named `screenshot.png`
* This image is a snapshot of what your custom theme looks like.

## Grab Images with the termial
* add-images-with-terminal.md

## Add special css comment to style.css
* Some may find this strange but WordPress using comments to give itself special instructions
* The `style.css` file is a perfect example of this
* Here you see a comment that adds meta information about the theme to the Dashboard
* You can put all your CSS in this file but there is a better way to break up your CSS with help from the `functions.php` file

1. Add the following comment code to the top of `style.css`
2. Name it `style.css`
3. **caution** If you name it something different like `styles.css` you will break your theme

`style.css`

```css
/*
Theme Name: Kingluddite Magazine Theme
Author: Kingluddite
Author URI: http://kingluddite.com/
Description: A simple theme to showcase how to build a theme from Twitter Bootstrap
Version: 1.0
License: GNU General Public License v2 or later
License URI: http://www.gnu.org/licenses/gpl-2.0.html

Code and take over the world line-by-line
*/
```

## Activate Your Theme
1. Log into the WordPress Dashboard and navigate to `Appearance > Themes`
  * You should see your theme
  * If you don't you may have created a `broken theme` and you'll have to troubleshoot to get it working
  * WordPress will give you a broken theme notice and let you know what the problem may be
  * If you added a `screenshot.png` to your theme, you will see that image in the themes page of your Dashboard

2. Click Activate and your theme will now be live.
3. Click `Visit Site` in Dashboard to view your live site Don't get too excited because it will blank

## A Better way to activate your theme using WP-CLI
`$ wp themes activate thunder-tube-theme`

## Let's Start building Our Bootstrap 4 Starter Template
* We'll use this template and convert it so it looks the same in our WordPress custom theme

[Our BS template](http://v4-alpha.getbootstrap.com/examples/jumbotron/)

## index.php
* This is the first file we will work with in our custom theme
* Copy the source from the Bootstrap 4 jumbotron example and paste it into `index.php`
* The CSS won't be working but you will see the content
* View the page in Chrome and view the source of the page

## Break up our page into pieces
* Instead of working with one large file we will break it into smaller, more manageble pieces

### header.php
* We want to grab the top part of our `index.php` page and put it inside `header.php`

#### Follow this instruction:
* In `index.php`, highlight from the `<!DOCTYPE html>` down to the closing `</nav>` element, cut it, and paste inside `header.php`

### php includes
* Our header is inside `header.php` (we cut and pasted it from `index.php`)
* We need a way of pointing `index.php` to the code inside `header.php`
* **PHP includes** will enable us to do this
* We will use: 
    - `get_header()` to grab our header content
    - and `get_footer()` to grab our footer content

#### get_header()
* If you worked with PHP before you know about includes
* They enable you to include a chunk of code onto another page
* `get_header()` will pull into `index.php` the code inside `header.php`.

### Follow this instruction:
* Replace the cut code in `index.php` with the following PHP code:

`index.php`

```php
<?php get_header(); ?>
[rest of index.php code here]
```

* View page in browser to test if header include is working

#### get_footer()
* Now we are going to use the same technique to break up our footer content into it's own file and we'll use the `get_footer()` include to pull it into `index.php`

##### Follow these instructions
* In `index.php` select and cut from the `HR` element to end of the `HTML` element and paste into `footer.php`.
* Replace cut code in `index.php` with the following PHP code:

`index.php`

```php
[rest of index.php code here]
<?php get_footer(); ?>
```

* View page in browser to test if `footer.php` include is working
* Now we have the exact same end result we had when all our code was inside `index.php` but the benefit of our new approach is that our code is more modular and maintainable

## Proper Way to Include CSS in WordPress
* In a static HTML page, you use `<link>` elements to point to the CSS files
* In WordPress the proper (and secure) way to include CSS is through the `functions.php` page.

## functions.php
* When we add our `theme_styles()` function it will give us the ability to line up all our css and dynamically inject them into the HTML that the PHP in WordPress dynamically creates

### Add the following to `functions.php`:

`functions.php`

* Make sure you are using the latest CDN URL from the [bootstrap 4 site](https://getbootstrap.com/docs/4.0/getting-started/introduction/)

```php
<?php
function theme_styles() {
    wp_enqueue_style( 'bootstrap_css', 'https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css' );
}

add_action( 'wp_enqueue_scripts', 'theme_styles' );
?>
```

* The `add_action` will tie the function to the `wp_enqueue_scripts` WordPress `hook`.
* **Note** Leave off the closing PHP in `functions.php` is a good practice

## What are Hooks in WordPress?
* [What are WordPress hooks?](https://www.smashingmagazine.com/2011/10/definitive-guide-wordpress-hooks/)

### hooks explained
* There will be times where you need to inject code at a certain part of your custom theme
* That is where `hooks` come in
    - In a static HTML site, you would put LINK elements to point to your CSS
    - In WordPress we use the `wp_head()` **hook** to inject the CSS we have line up (WordPress calls this lining up **enqueuing**)
* Once you do this and view your WordPress site, you will see that the CSS from Twitter Bootstrap's Jumbo sample page is now working

## Follow this instruction:
* In `header.php` add this PHP before the closing HEAD element

`header.php`

```php
<?php wp_head(); ?>
</head>
```

## View Source Code
* When we first start building themes in WordPress we need to get into the habit of checking if our page is generating errors
* The Chrome inspector tool will help us find errors.

### 404 Errors
* Here are some 404 errors when we use the Console in Google Chrome (shortcut to open is `cmd`+`option`+`j`)
* This will let you see if the hook is working

### Our Bootstrap Jumbotron code still has static links
* Check out all these broken 404 pages
* It means we requested the page from the server and the server tells us the files we requested do not exist on the server

![404 errors](https://i.imgur.com/6RW0uHc.png)

* You should now see `bootstrap.min.css` in your inspector

![our hook is working](https://i.imgur.com/3jC4etb.png)

## Clean up time
* Delete unused old link to `bootstrap.min.css`
    - We don't need this anymore because we will be dynamically injecting this `<link>` using our new function in `functions.php` and the hook that will inject it above our closing `</head>`

## Add more of our CSS using functions.php
* What about the link to `jumbotron.css`? 
* How can we add links to our custom CSS when using WP?

### Follow these instructions
* On the original static source if you click on the link it will take you to `jumbotron.css` and show you this code:

`jumbotron.css`

```css
body {
  padding-bottom: 2rem;
}
```

1. Copy that code
2. Create a new file called `jumbotron.css`
3. Put it inside a new folder called `css`

### Enqueue our new css files
* To enqueue it (add it to our list of needed css files) adjust our `functions.php` file to look like the following:

`functions.php`

```php
/* add css */
function theme_styles() {
    wp_enqueue_style( 'bootstrap_css', get_template_directory_uri() . '/node_modules/bootstrap/dist/css/bootstrap.min.css' );
    wp_enqueue_style( 'jumbotron_css', get_template_directory_uri() . '/css/jumbotron.css' );
    wp_enqueue_style( 'main_css', get_template_directory_uri() . '/style.css' );
}
add_action( 'wp_enqueue_scripts', 'theme_styles' );
?>
```

* Above shows you how we can point to our `node_modules` version of bootstrap 4, as well as a custom CSS file `jumbotron.css` 
* and we also can point to the `style.css` file located in the root of our WordPress theme

## Congrats! 
* Now you know how to add CSS files into your WordPress theme

### Clean up time
* You can now delete the "hardcoded" `<link>` tag inside your `header.php` file that points to `jumbotron.css`

`header.php`

```html
    <!-- DELETE THIS LINE in header.php -->
    <link href="jumbotron.css" rel="stylesheet">
```

* **note** `style.css` is a special file for WordPress
* You must name the file `style.css` and you must place it in the root of your theme
* It also needs the special meta data comments.

#### wp_enqueue_style('name', path to file) syntax
* The first parameter is an internal name WordPress uses
* The second parameter concatenates a cool `get_template_directory_uri()` function that has the ability to point us the active theme and then we concatenate the rest of the path from inside the custom theme folder
* If you view the source you should now see that our custom theme `style.css` file is now there:

![style.css in source code](https://i.imgur.com/9QSoFEG.png)

### Did you know?
* WordPress has jQuery built in

## Adding JavaScript using functions.php
`functions.php`

`wp_enqueue_script` which has different parameters than `wp_enqueue_style`

### Important parameters in wp_enqueue_script()
* The 3rd parameter allows us to include dependencies this JavaScript file needs
* It can be **one** file (like jQuery) **or more than one** file
* The last parameter is Boolean which enable you to have:
    - The JavaScript file in the `<head>` element of our `header.php` (false)
    - Or in the `footer.php` file before the closing `</body>` tag (true)

`functions.php`

```php
// MORE CODE
function theme_js() {
  wp_enqueue_script( 'tether_js', get_template_directory_uri() . '/assets/js/tether.min.js', '', '', true );
    wp_enqueue_script( 'bootstrap_js', get_template_directory_uri() . '/assets/js/bootstrap.min.js', array('jquery'), '', true );
}
add_action( 'wp_enqueue_scripts', 'theme_js' );
// MORE CODE
```

## Another hook - wp_footer()
* Hooks are essential in WordPress
* The `wp_footer()` gives us the ability to inject code before the closing of `</body>`

### View source
* You won't see included `bootstrap.min.js` in footer
because you forgot to include the footer hook

`footer.php`

```php
[more code here]
<?php wp_footer(); ?>

 </body>
</html>
```

### Delete JavaScript static code located in `footer.php`
* Notice that as we enqueue CSS and JavaScript we need to delete all the hard coded `<link>` and `<script>` tags in our `header.php` and `footer.php` files

**note** your code may be slightly different but make sure you delete any hardcoded links to JavaScript

`footer.php`

* Delete all the code below:

```html
 <!-- Bootstrap core JavaScript
    ================================================== -->
    <!-- Placed at the end of the document so the pages load faster -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
    <script>window.jQuery || document.write('<script src="../../assets/js/vendor/jquery.min.js"><\/script>')</script>
    <script src="../../dist/js/bootstrap.min.js"></script>
    <!-- IE10 viewport hack for Surface/desktop Windows 8 bug -->
    <script src="../../assets/js/ie10-viewport-bug-workaround.js"></script>
```

## WordPress filters

### Hide admin bar
* Sometimes when logged in, the WordPress Admin CSS gets in your way
* You have the ability to override it

`functions.php`

```php
add_filter( 'show_admin_bar', '__return_false' );
```

* This is a simple filter that hides the admin bar

## body_class()
* This is a powerful dynamic function that generates a bunch of classes inside the `<body>` element
* You can use these classes to style a WordPress page the way you want depending on the situation
* **tip** In your final project you will use a class generated to style the images located inside the `domsters` header
* If you add the following code and then view the source code, you'll see a bunch of classes added to the body

`header.php`

```php
<body <?php body_class(); ?>>
<!-- rest of code -->
```

![body class php function output](https://i.imgur.com/5SljR6O.png)

### Push down the admin bar when logged in
* `body_class()` gives you the ability to style the admin panel without affecting the public facing UI of your site
* Add this underneath our existing code

`style.css`

```css
.admin-bar .navbar.fixed-top {
    margin-top: 30px;
}
```

## Permalinks
* Remember that when you first create your WordPress site, go into the Dashboard and reset your Permalinks
* This will regenerate your `.htaccess` file
* **Troubleshooting Tip** When your site links are not working make this your first troubleshooting step
* This a great feature of WordPress that will improve your SEO with a couple clicks
* It makes your URL much more SEO friendly

### Before Permalinks
![Before Permalinks](https://i.imgur.com/zukfE79.png)

### After Permalinks
![After Permalinks](https://i.imgur.com/gRCsfkk.png)

## Install Bootstrap Shortcodes
* This will let your client to add Bootstrap code without having to type it
* Remember that Bootstrap 3 and 4 use different classes Make sure this plugin is the one you want. You may need to find a more updated plugin for Bootstrap 4 snippets.
* [link](https://wordpress.org/plugins/bootstrap-shortcodes/)
* Kevin Attfield
* Install and Activate
* view page and you'll see shortcodes in editor

## Add Google Fonts
[What is the proper way to use Google fonts with WordPress?](http://www.wpbeginner.com/wp-themes/how-add-google-web-fonts-wordpress-themes/)

* Add this code:

`functions.php`

```php
// activate google fonts
function tutsplus_add_google_fonts() {
  wp_register_style( 'googleFonts', 'http://fonts.googleapis.com/css?family=Open+Sans:400,300');
  wp_enqueue_style( 'googleFonts');
}
add_action( 'wp_enqueue_scripts', 'tutsplus_add_google_fonts' );
```

And here is an example:

`style.css`

```css
/* Headings */
h1, h2, h3, h4, .site-name {
  font-family: 'Open Sans', sans-serif;
}
```

# WordPress Notes

* [part 1](bootstrap-wordpress-part-1.md)
* [part 2](bootstrap-wordpress-part-2.md)
* [part 3](bootstrap-wordpress-part-3.md)
* [part 4](bootstrap-wordpress-part-4.md)
* [part 5](bootstrap-wordpress-part-5.md)
