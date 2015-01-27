# System Requirements

* Web server with URL rewriting
* PHP 5.4.0 or newer

# Install with Composer

The preferred installation method is [Composer](https://getcomposer.org/). Navigate into your project directory and execute the following bash command. This command downloads the Slim Framework into your project's `vendor/` directory and creates the appropriate autoloader.

    composer require slim/slim

Next, require the Composer autoloader into your PHP script.

    <?php
    require 'vendor/autoload.php';

# Install Manually

You can install the Slim Framework without Composer. Download the Slim Framework files into your project directory. Next, require the `Slim/Autoloader.php` file into your PHP script and invoke its static `register()` method.

    <?php
    require 'Slim/Autoloader.php';
    \Slim\Autoloader::register();
