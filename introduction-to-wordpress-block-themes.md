# Introduction to Block Themes in WordPress

After working with classic themes from WordPress, I decided to try out the "New Way of Doing Things on WordPress": **_The WordPress Block Themes._**

Block themes were introduced in WordPress 5.9 and has become stable in the following versions. At it's core, a block theme uses WordPress Blocks for every part of the WordPress site. According to a [support article by WordPress](https://wordpress.org/documentation/article/block-themes/):
> These themes are built for the newest features coming to WordPress that allow you to edit and customize all parts of your site.

The block themes allow users to customize every part of the website using the Site Editor. Through the site editor, the user can access all parts used to customize the different templates on their website.

### Benefits of using block themes
* Better performance - Styles are only loaded when the block using them is loaded.
* User-friendliness - Block themes allow users to customize every part of their website without using code.
* Enqueue stylesheets not required - `Block themes are not required to manually enqueue stylesheets for both front-end and editors`
* Enhanced accessibility - No code required to create basic accessibility features including the "Skip to content" option, navigation using the keyboard, and other important landmarks which are automatically created.
* By using the Styles interface, users can easily customize the colors and typography for their entire website and for the individual blocks used within it.
* 'Welcome, Theme.json' - Exists add_theme_support(). The theme.json file takes care of everything related to supporting the theme.

----
## From WordPress Docs
#### **What options are available with block themes?**
After activating a block theme, more tools and features will be available to you including the following:

- *Site Editor*: an editor that allows you to edit all parts of your site, navigate between templates, and more.
- *Styles*: a feature that allows you to customize your site, including individual blocks, as much as you’d like with different colors, typography, layouts, and more.
- *Templates*: edit, create, and manage templates that a page or post uses.
- *Template parts*: a way to organize and display groups of blocks as part of a block template mainly for site structure, like Headers and Footers.
- *Theme blocks* including the Navigation Block, Query Loop block, and more.
----
### How are block themes different? *(from classic themes)*
Block themes are specifically designed for the newer features in WordPress. They let you use blocks to edit every aspect of your website, making it easier to customize things like background colors, font sizes, and Heading blocks.

On the other hand, classic themes are created using PHP templates and functions.php, and they come with features such as Widgets, a separate Menus section, the Customizer, and more. However, classic themes cannot be used with the Site Editor.

Check out this [comparison table by WordPress Docs _*HERE*_](https://developer.wordpress.org/themes/block-themes/#differences-and-similarities-between-classic-themes-and-block-themes).

## References
1. [Block Themes - Support article](https://wordpress.org/documentation/article/block-themes/)
2. [Block themes - Developers Guide](https://developer.wordpress.org/themes/block-themes/)
