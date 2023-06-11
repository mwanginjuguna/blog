# Widgets and Sidebar in WordPress Theme Development
A widget in WordPress is a php obj that outputs some html. A widget:
- can be used many times
- can save data to db
- wp includes several default widgets
- can use theme or plugin to register widgets

A Sidebar is used to add widgets.
Widgets are added in the sidebar.
Users use sidebars to customize their site.
- A sidebar can appear on one page or multiple pages
- Adding sidebar is optional; but it allows users to add content to widget areas in customizer.

## Registering Sidebar
* register_sidebar() - single sidebar
* register_sidebars() - multiple sidebars

## Displaying sidebars
* sidebar.php / dynamic_sidebar() - sidebar template
* sidebar-{name}.php - custom
* get_sidebar() - display the sidebar

## Developing custom widgets
- create a widget class that extends WP_Widget
* common functions include:
  
`construct`,

``widget($args, $instance) {// outputs the content of widget}``,
      
``form($instance) {// outputs the options form in the admiin}``,
      
``update($new_instance, $old_instance) {//process widget options to be saved to db in options table}``  
- register widget

````php
// register widget
add_action('widgets_init', 'my_widget');

function my_widget() {
    register_widget('My_Class_Widget');
}
````
- make sure you have already create the sidebar.
