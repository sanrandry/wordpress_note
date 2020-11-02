---
title: Start here
description: ""
position: 2
category: Guide
---

## create setup function
```php[functions.php]
your_theme_name_setup() {

}
add_action('after_setup_theme', 'rakitra_janga_setup');
```
## include assets
```php[functions.php]

function add_theme_scripts() {
  wp_enqueue_style( 'style', get_stylesheet_uri() );
 
  wp_enqueue_style( 'slider', get_template_directory_uri() . '/css/slider.css', array(), '1.1', 'all');
 
  wp_enqueue_script( 'script', get_template_directory_uri() . '/js/script.js', array ( 'jquery' ), 1.1, true);
 
    if ( is_singular() && comments_open() && get_option( 'thread_comments' ) ) {
      wp_enqueue_script( 'comment-reply' );
    }
}
add_action( 'wp_enqueue_scripts', 'add_theme_scripts' );
```
## create header
create a header.php file and put all you header there  
this is an example from underscore based theme
```php[header.php]
<?php

/**
 * The header for our theme
 *
 * This is the template that displays all of the <head> section and everything up until <div id="content">
 *
 * @link https://developer.wordpress.org/themes/basics/template-files/#template-partials
 *
 * @package madava
 */

?>
<!doctype html>
<html <?php language_attributes(); ?>>

<head>
	<meta charset="<?php bloginfo('charset'); ?>">
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<link rel="profile" href="https://gmpg.org/xfn/11">

	<?php wp_head(); ?>
</head>


<body <?php body_class(); ?>>
	<?php wp_body_open(); ?>
	<div id="page" class="site hide">
````
## create footer.php
footer example file from underscore based thme
```php[footer.php]
<?php

/**
 * The template for displaying the footer
 *
 * Contains the closing of the #content div and all content after.
 *
 * @link https://developer.wordpress.org/themes/basics/template-files/#template-partials
 *
 * @package madava
 */

?>
</div><!-- #page -->

<?php wp_footer(); ?>

</body>
```
## custom logo suupport
add this to functions.php inside the setup function
```php[functions.php]
add_theme_support(
			'custom-logo',
			array(
				'height'      => 250,
				'width'       => 250,
				'flex-width'  => true,
				'flex-height' => true,
			)
		);

```
to get the custom logo url, proced like this
```php
<?php
  $logo = get_theme_mod('custom_logo');
  $logo_url = wp_get_attachment_image_src($logo, 'full')[0];
 ?>
```
## register a navmenu
add this in the functions.php inside the theme setup function
```php[functions.php]
// This theme uses wp_nav_menu() in one location.
function yourtheme_register_nav_menu() {
	register_nav_menus(
		array(
			'menu-1' => esc_html__('Primary', 'rakitra-janga'),
			)
	);
}
add_action('init', 'yourtheme_register_nav_menu');
```
to display the navbar
first, crate a menu and add an item to this menu an active the custom menu location
add those line of code on the template were you like to display the menu
```php
<?php
wp_nav_menu(
array(
		'theme_location' => 'menu-1'
	)
);
?>
```
if your need is out of the default nav menu
read this aritcle to create a custom nav menu walker: https://gist.github.com/sanrandry/772ff35356e54dac5c35c8cd22982cfb
# add active link custom class
add this to functions.php file
```php[functions.php]
add_filter('nav_menu_css_class' , 'special_nav_class' , 10 , 2);
function special_nav_class($classes, $item){
     if( in_array('current-menu-item', $classes) ){
             $classes[] = 'active ';
     }
     return $classes;
}
```
## custom home page
create an front-page.php file and add you home page code there
## page template creation
create a new file in the them racine
include this comment in the top of the file
```php
/*
Template Name: Special Layout
*/
```
## child page menu
```php[functions.php]
// get top ancestor
function get_top_ancestor_id() {
  global $post
  if($post->post_parent) {
    $ancestors = array_reverse(get_post_ancesstors($post->ID));
  }
  return $post->ID;
}
```
```php[page.php]
 wp_list_pages(["child_of" => $post->ID, "title" => ''])
```
## post meta data
display the time:
```php
<?php the_time(); ?>;
```
display the author:
```php
<?php the_author(); ?>;
```
display the  url:
```php
<?php the_author_posts_url(get_the_author_meta("ID")); ?>;
```
display the categories:
```php
<?php get_the_category(); ?>;
```
## the excerpt function
change the_content to to the_excerpt functionor get_the_excerpt function in your post page.
### custom excerpt leght
add this to fuctions.php file
```php[functions.php]
function custom_excerpt_leght($length) {
return 80;
}
add_filter(‘excerpt_length’, ‘my_excerpt_length’);
```
## featured image
add this code to the functions.php
```php[functions.php]
function the_theme_setup() {
  add_theme_support("post-thumbnails");
}
add_action("after_setup_them", "the_them_ssteup");
```
### output the image
```php 
 <?php 
  the_post_thumbnail();
  // add additional image size
  add_image_size("small-image", 180, 120 true);
  add_image_size("banner-image", 920, 210 true);
  
```
## search
add the default sarch form
```php
<?php get_search_form() ?>
```
custom search form
create a new file in the theme folder name searcform.php
copy the wordpress default search form code
```php[search.php]
<form action="/" method="get">
    <label for="search">Search in <?php echo home_url( '/' ); ?></label>
    <input type="text" name="s" id="search" value="<?php the_search_query(); ?>" />
    <input type="image" alt="Search" src="<?php bloginfo( 'template_url' ); ?>/images/search.png" />
</form>
```
customize search result
create a file search.php and put the customized result there.
## get_template_part
create a file of the name of the template and call get_template_part with this file name
```php
get_template("template_name");
```
## post format
there is 9 standared post format
 add this line to functions.php
 ```php[functions.php]
 add_theme_support("post-formats", ["aside", "gallery", "link");
 ```
when get the post template, get it with the post format
 ```php
 get_template_part("template_name", get_post_format());
 ```
## widgets
create a function to add the widget location in the function.php file
 ```php[functions.php]
 function theme_widget_init() {
  register_sidebar(["name" => "Sidebar", "id" => "sidebar1"]);
}
add_action("widgets_init", "theme_widget_init");
 ```
display the widget location
 ```php
 dynamic_sidebar("sidebar1");  
 ```
### create a custom widget
 user this documment to create a custom widget: https://gist.github.com/sanrandry/d9f542413cee911fec38f8a8208e3f02
 ## custom home page
 create a file named front-page.php
 ## WP_Query
 ```php
  $custom_post = new WP_Query(["cat" => "7", "posts_per_page" => "2"]);
  if($custom_post->have_posts()): 
    while($custom_post->have_posts()):
    $custom_post->the_post();
    the_title();
    endwhile;
  endif;
  wp_reset_postdata();
  
 ```
 ## customizer api
 ```php function.php
 function test_customize_register($wp_customize){
  // add a panel  this is note required
 $wp_customize->add_panel( 'panel_id', array(
  'title' => __( 'my panel' ),
  'description' =>  __("the section description"), // Include html tags such as <p>.
  'priority' => 160, // Mixed with top-level-section hierarchy.
) );
 // add section
 $wp_customize->add_section("section_id", [
  "title" => __("section title"),
  "description" => __("the section description"),
  "priority" => 130,
  'panel' => 'panel_id' // if you like to use a panel
 ]);
 
 // then add setting
 $wp_customize->add_setting("banner_heading", [
  "default" => __("Default value for the setting"),
  "type" => "theme_mod"
 ]);
 // then add a control the this setting
 $wp_customize->add_control("banner_heading", [
  "label" => __("Banner heading"),
  "section" => "showcase",
  "priority" => 1
 ]);
 
 }
 add_action('customize_register', 'test_customize_register');
 
 ```
 to display the setting, use the get_thme_mod function in the template
 ```php
 echo get_them_mod("setting_id", "default value");
 ``` 
 
 this is ome more customizer example for image customization
 ```php
 // the image setting
 $wp_customize->add_setting("your_setting_id", [
        "default" => get_bloginfo('template_directory') . '/assets/images/pexels-jeshootscom-442574.jpg',
        "type" => "theme_mod"
    ]);
  // the image control
 $wp_customize->add_control(
       new WP_Customize_Image_Control(
           $wp_customize,
           'logo',
           array(
               'label'      => __( 'Upload a logo', 'theme_name' ),
               'section'    => 'your_section_id',
               'setting'   => 'your_setting_id'
           )
       )
   );
 ```
 ## post type
 register a new post type
 ```php
 function your_theme_regsiter_post_type() {
    register_post_type('id', [
        'label' => 'foo',
        'public' => true,
        'menu_position' => 3,
        'menu_icon' => 'dashicons-building', // you can wordpress building icon: https://developer.wordpress.org/resource/dashicons/#code-standards
        'supports' => ['title', 'editor', 'thumbnail', 'excerpt'],
        'show_in_rest' => true,
        'has_archive' => true,
    ]);
}
add_action('init', 'your_theme_regsiter_post_type');
 ```
 then user QP_Query to displplay the post type
 ## pagination
 use the wordpress build in function
 ```php 
 echo(paginate_links());
 ```
 to customize this a pagination, create a function like this
 ```php [function.php]
 function wpdocs_get_paginated_links( $query ) {
    // When we're on page 1, 'paged' is 0, but we're counting from 1,
    // so we're using max() to get 1 instead of 0
    $currentPage = max( 1, get_query_var( 'paged', 1 ) );
 
    // This creates an array with all available page numbers, if there
    // is only *one* page, max_num_pages will return 0, so here we also
    // use the max() function to make sure we'll always get 1
    $pages = range( 1, max( 1, $query->max_num_pages ) );
 
    // Now, map over $pages and return the page number, the url to that
    // page and a boolean indicating whether that number is the current page
    return array_map( function( $page ) use ( $currentPage ) {
        return ( object ) array(
            "isCurrent" => $page == $currentPage,
            "page" => $page,
            "url" => get_pagenum_link( $page )
        );
    }, $pages );
}
```
and use it like this, (I am using uikit pagination in this exemple)
```php
<?php $pagination_array = rossignol_inside_get_paginated_links($wp_query); ?>
<?php if (count($pagination_array) > 1) : ?>
	<ul class="uk-pagination uk-flex-center" uk-margin>
		<?php $previous_page_link = get_previous_posts_page_link(); ?>
		<?php $next_page_link = get_next_posts_page_link(); ?>
		<?php if ($previous_page_link != home_url($wp->request) . '/') : ?>
			<li><a href="<?php echo ($previous_page_link) ?>"><span uk-pagination-previous></span></a></li>
		<?php endif; ?>
		<?php foreach ($pagination_array as $link) : ?>
			<li class="<?php if ($link->isCurrent) {echo('uk-active');} ?>">
				<a href="<?php echo ($link->url) ?>"><?php echo ($link->page) ?></a>
			</li>
		<?php endforeach; ?>
		<?php if ($wp_query->max_num_pages != get_query_var('paged')) : ?>
			<li><a href="<?php echo ($next_page_link) ?>"><span uk-pagination-next></span></a></li>
		<?php endif; ?>
	</ul>
<?php endif ?>
```
 ## security
 ### first add this line in wp_config.php
 this line prevent the ftp login requirement when install a plugin or theme
 ```php[wp_config.php]
 define('FS_METHOD','direct');
 ```
 prevent directory browsing  
 add this line to the .htacess file
 ```
  # BEGING  directory browsing block
  options -Indexes
  # END directory brosing block
 ```
 change wp-content, uploads and wp-iclude directory
 use this plugin: WP Hide & Security Enchancer
 https://www.wp-hide.com/  
 hide login page  
 use this two plugin: WPS hide my login and Limit Login Attemps Reloaded  
 if you like to block certain IP use: Wordfense Security - Firewall & Malware Scan
 
