## Using PHP's OOP to bootstrap and autoload functionality in WordPress Custom Theme
-- --
**
* In PHP, Object-Oriented Programming provides several fundamental practices that improve code modularity and reusability including (but not limited to) traits and singletons.
* Traits, in PHP, allow the grouping of common behaviors or functions/methods. These grouped functions can then be inherited to other classes as needed.
* Singleton on the other hand, is a design pattern that ensures a class is only instantiated once in the entire application. This ensures only a single instance of the class is initialized.
----
In WordPress, OOP concepts and practices increase code reusability and optimize the performance of the WordPress application.
During Theme development, bootstrapping theme functionality helps in separating the functionality of the WordPress core from the theme and also from plugins.

<p>A recommended best practice is to work hard to eliminate conflict of interests between the theme and other functionalities such as WordPress core and plugins. This is because other functionalities outside the theme could be updated, changed, discontinued or removed causing theme to crash. To ensure theme functionality remains independent and isolated, bootstrapping is essential.</p>

Using classes, singleton and autoloader, a developer can bootstrap core theme assets and functions using a single class. 
#### Autoloader
Autoloading helps in instantiating theme classes when they are required instead of having to manually `require` them throughout the theme. It helps initialize theme files and assets needed for dependency injection. (See php docs on spl_autoload_functions())[https://www.php.net/manual/en/function.spl-autoload-functions.php]

#### singleton
Utilizes trait to ensure classes are initialized only once in a specific theme instance.
```php
/**
* This singleton ensures a class is not instantiated more than once.
* @package Writer X
*/

namespace CUSTOMTHEME\Inc\Traits;
 trait Singleton {
    public function __construct()
    {
    // constructor
    }
    
    /**
    * Ensure class is only instantiated once.
    */
    final public static function getInstance()
    {
        static $instance = [];
        
        $calledClass = get_called_class();
        
        if (!isset($instance[ $calledClass ])) {
            $instance [ $calledClass ] = new $calledClass();
            
            do_action(sprintf('writing_theme_singleton_init%s', $calledClass));
        }
        
        return $instance[ $calledClass ];
    }
}
```

This singleton ensures that each class that inherits it(the singleton) is only instantiated once and can be accessed using getInstance() method

#### namespacing
Name spaces allow for multiple classes, and functions to be bundled together; say in the same folder/directory.
namespace CustomTheme\Inc
Adding this line on top of every file in the namespace ensures they can be used as if they were in the same file. All classes and functions defined in the namespace can be imported anywhere by using the namespace. For example:

## References
* [php docs on spl_autoload_functions()](https://www.php.net/manual/en/function.spl-autoload-functions.php)
* [Using namespaces and autoloading with composer in wordpress plugins by *Spencer Feng*](https://spencerfeng.medium.com/using-namespaces-and-autoloading-with-composer-in-wordpress-plugins-72ea36d82369)
