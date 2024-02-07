# WordPress Meta-boxes: How to optimize your website using the custom meta-boxes in WordPress

WordPress meta-boxes are used to create more additional fields related to the post. A common use case is giving users with permission to create posts to select whether or not they want to display the post title on the front page.

A useful use-case is SEO optimization of code to allow post-author add more information that can be embedded as meta tag of the post. For example include some key-words and other relevant details that can help web crawlers discover the content within the post or page. This is a functionality highly used by seo-plugins. 

### To add a new meta box, use the add_meta_box() function and plug it's execution to the add_meta_boxes action hook as follows:

Action-hook and the custom meta box:
````php 
add_action('add_meta_boxes', 'add_custom_meta_box');

/** 
* hide title on all Post post-types in wordpress
*/
function add_example_custom_box() 
{
	$screens = [ 'post']; // post type -  if you have custom post-types they would go here.

        foreach ($screens as $screen) {
            add_meta_box(
                'hide-page-title', // unique id
                __('Hide page title', 'write-x'), // Box title
                [$this, 'custom_meta_box_html'], // Content callback, must be of type callback
                $screen, // Post type
            'side' // context - show on side
            );
        }
}
````

The following custom_meta_box_html is the callback to retrieve the html template to render the meta box

````php 
/**
 * Html template to Allow user to show/hide post title in the editor
 * @param $post
 * @return void
 */
    public function custom_meta_box_html($post)
    {
        $value = get_post_meta($post->ID, '_hide_post_title', true); // this allows for the use of the saved value of the meta-box in other functionalities.

        ?>
        <label for="write_x_field"><?php esc_html_e('Hide the page title', 'write-x'); ?></label>
        <select name="write_x_field" id="write_x_field" class="postbox">
            <option value=""><?php esc_html_e('Select...', 'write-x'); ?></option>
            <option value="hide" <?php selected($value, 'hide') ?> >
                <?php esc_html_e('Hide', 'write-x'); ?>
            </option>
            <option value="show" <?php selected($value, 'show') ?> >
                <?php esc_html_e('Show', 'write-x'); ?>
            </option>
        </select>
<?php
    }
````

> You can add as many meta data as you want to any single post-type using the above functionality.

### To save a meta box, use the following function. 
```php
/**
     * Save the meta data to the database
     * @param
     * @return void
     */
    public function save_custom_post_meta_data($post_id)
    {
        if ( array_key_exists( 'write_x_field', $_POST ) ) {
            update_post_meta(
                $post_id,
                '_hide_post_title',
                $_POST['write_x_field']
            );
        }
    }
```

### Finally add nonce verification
Nonces protects website from csrf through hashing or csrf tokens.
* Creating a nonce: wp_nonce_url() to url, wp_nonce_field() to add to a form, wp_create_nonce() custom way
* Verifying a nonce(): check_admin_referrer() for admin screen (came from url), check_ajax_referrer(), cheks nonce(not referrer), wp_verify_nonce() checks for nonce from other contexts

Example you can use nonce in a custom form by using ``<?php wp_nonce_field('name_of_action', 'name_of_nonce_field'); ?>`` inside the form. This will create a hidden input element inside the form containing the csrf token.

On the back-end, you can check whether the post request contains the csrf through wp_verify_nonce():

````php
if( !isset( $_POST['name_of_nonce_field']) || !wp_verify_nonce($_POST['name_of_nonce_field'], 'name_of_my_action') {
	// handle failed verification
} else {
	// process data from form
}
````
## References

- [Custom Meta-Boxes - https://developer.wordpress.org/plugins/metadata/custom-meta-boxes/](https://developer.wordpress.org/plugins/metadata/custom-meta-boxes/)
- #37 WordPress Nonce Example | WordPress Nonce Ajax | wordpress nonce verification | verify_nonce[https://youtu.be/3mtkI6ibWRc](https://youtu.be/3mtkI6ibWRc) by [Imran Sayed - Codeytek Academy](https://www.youtube.com/@Codeytek)

