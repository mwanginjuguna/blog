# How to Customize Featured Images and Post Thumbnails in WordPress
## How to enable featured images in posts and pages:
WordPress requires the theme developer to explicitly enable support for featured posts. To enable support for featured images or thumbnails
1. Open your functions.php file, or the theme class that you use to setup or bootstrap theme functionality.
2. Call this wordpress function:
    ``add_theme_support( 'post-thumbnails' );``
The add_theme_support( ) function registers theme support for a given feature. According to wordpress:
The function "Must be called in the theme’s functions.php file to work. If attached to a hook, it must be ‘after_setup_theme’. The ‘init’ hook may be too late for some features."

``add_theme_support( string $feature, mixed $args ): void|false``

### Use the following functions to interact with the image inside the theme:
```
add_image_size() – Register a new image size.
set_post_thumbnail_size() – Registers an image size for the post thumbnail.

has_post_thumbnail() – Check if post has an image attached.
the_post_thumbnail() – Display Post Thumbnail.

get_the_post_thumbnail() – Retrieve Post Thumbnail.
get_post_thumbnail_id() – Retrieve Post Thumbnail ID.
```
the_post_thumbnail() function accepts the first parameter as the size of the image. Eg. the_post_thumbnail(size: [200, 200]) // this assigns the image width and height of 200px.

### Wordpress provides default image sizes as follows:
```php 
the_post_thumbnail(); // Without parameter ->; Thumbnail
the_post_thumbnail( 'thumbnail' ); // Thumbnail (default 150px x 150px max)
the_post_thumbnail( 'medium' ); // Medium resolution (default 300px x 300px max)
the_post_thumbnail( 'medium_large' ); // Medium-large resolution (default 768px x no height limit max)
the_post_thumbnail( 'large' ); // Large resolution (default 1024px x 1024px max)
the_post_thumbnail( 'full' ); // Original image resolution (unmodified)
the_post_thumbnail( array( 100, 100 ) ); // Other resolutions (height, width)
```

#### Basic Usage:
```php
// check if the post or page has a Featured Image assigned to it.
if ( has_post_thumbnail() ) {
    the_post_thumbnail();
}
```

## Reference
*[add_theme_support() - https://developer.wordpress.org/reference/functions/add_theme_support/](https://developer.wordpress.org/reference/functions/add_theme_support/)
