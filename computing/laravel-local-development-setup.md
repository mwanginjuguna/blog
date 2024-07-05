# Setting up the local environment for Laravel development

Laravel community is growing fast. Now more than ever, developers from varying experience groups are finding Laravel as the go-to resource for building applications.

One of the main reasons why Laravel is gaining popularity is the ease of configuration and a vibrant ecosystem of first-party and third-party packages. Taylor Otwell, the creator of Laravel, together with the core Laravel developers, have worked really hard to make Laravel super easy to get started.

In a recent podcast, Taylor said that the goal is to reduce the friction of configuration and setting up the Laravel environment. This means that developers from other languages and frameworks as well as beginners, can easily install and start using Laravel without the hustle of having to install a lot of dependencies.

So, what options are available for setting up a productive local environment to start working with Laravel? After working with Laravel for over 3 years, I tried to answer this question. Here is what I found.

Let's dive in.

## 1. Zero configuration

> The basic prerequisites of Laravel are **php and composer**. With these installed, you can run `laravel new example-project` to create a new Laravel application.

Before Laravel 11 was released (2024), in addition to installing Laravel, you also needed to configure a database connection. This would involve setting up a local database such as MySQL or PostgreSQL.

Laravel 11 introduced SQlite as the default database connection for all new Laravel applications. So, when you run `laravel new example-project` on your terminal you get a list of options with SQlite as the default option. This also means that you start buidling your application immediately without wasting time setting up the database.

Over the years, Laravel core team working with Taylor remains dedicated to making it very easy to start using Laravel. This zero-configuration approach means that once you have started a new application, you jump right into writing application logic. SQlite is automatically configured and all migrations are run during installation.

**The main drawback with this approach is the infamous *"it works on my computer"* problem.** While working with a team, this approach can lead to problems. It is difficult to keep the teams in sync because dependencies for specific local environments and variables can be difficult to track.

![](https://github.com/mwanginjuguna/public-image-assets/blob/main/blog/laravel-local-development-tools.png?raw=true)

## 2. Laravel Herd

Unlike `Homestead` and `Sail` which rely on *a virtual machine* and *Docker*, Herd installs directly on your PC (Windows and MacOS). This eliminates the overhead of managing a separate virtual machine and Docker, and offers a more lightweight and resource-efficient solution.

[Herd](https://herd.laravel.com/) is a blazing fast, native Laravel and PHP development environment for Windows. It includes everything you need to get started with Laravel development, including PHP and nginx. Once you install Herd, you're ready to start developing with Laravel.

Herd provides a simple UI that allows you to manage various aspects of your development environment. You can use the UI for:

* Starting and stopping Herd services (PHP, Nginx, etc.)
* Switching between different PHP versions
* Viewing logs
* Managing parked directories and project access

Herd is the easiest way to setup Laravel development environment. The only drawback is team members might have slightly different Herd configurations on their local machines, leading to potential inconsistencies in development environments. However, Herd provides a `Herd Teams` license for your whole team and assign licenses to your team members in the license management portal.

Checkout [Herd official documentation for more information](https://herd.laravel.com/).

## 3. Docker configuration with Laravel Sail

> Prerequisites: Docker.

[Laravel Sail](https://laravel.com/docs/11.x/sail) is a light-weight command-line interface for interacting with Laravel's default Docker development environment. Sail provides a great starting point for building a Laravel application using PHP, MySQL, and Redis without requiring prior [Docker](https://www.docker.com/get-started/?ref=mwangikanothe) experience.

Under the hood, Sail is the `docker-compose.yml` file and the `sail script` that is stored at the root of your project. The sail script provides a CLI with convenient methods for interacting with the Docker containers defined by the `docker-compose.yml` file.

Laravel Sail is supported on macOS, Linux, and Windows (via [WSL2](https://docs.microsoft.com/en-us/windows/wsl/about)).

Laravel Sail provides a convenient way to get started with Docker for Laravel development. It uses Docker Compose under the hood to define a multi-container environment with all the necessary services (PHP, MySQL, Redis) pre-configured for your Laravel application.

Laravel Sail is automatically installed with all new Laravel applications so you may start using it immediately.

If you are interested in using Sail with an existing Laravel application, you may simply install Sail using the Composer package manager. Of course, these steps assume that your existing local development environment allows you to install Composer dependencies:

```bash
composer require laravel/sail --dev
```

After Sail has been installed, you may run the `sail:install` Artisan command to publish Sail's `docker-compose.yml` file and modify your .`env` file with the required environment variables for connecting to Docker Services.

[Read more about using Sail in Laravel from official docs](https://laravel.com/docs/11.x/sail).

**The key advantage to using Docker and Sail is improved harmony in team projects.** All members of the team can setup and configure Docker continers for the specific project. This allows everyone to be in sync and use the same dependencies and environment variables.

## 4. Virtualization (VM) with Laravel Homestead

> Prerequisites: [Vagrant](https://developer.hashicorp.com/vagrant/downloads) and [VirtualBox](https://www.virtualbox.org/)

[Laravel Homestead](https://laravel.com/docs/11.x/homestead) is an official, pre-packaged [Vagrant box](https://app.vagrantup.com/laravel/boxes/homestead) that provides you a wonderful development environment without requiring you to install PHP, a web server, or any other server software on your local machine. Vagrant is a tool for managing virtual machines. Laravel Homestead is a pre-configured virtual machine that provides a development environment specifically designed for Laravel applications.

Once you install Homestead, you don't need to install any other program. Supported on any Windows, macOS, or Linux system, Homestead includes Nginx, PHP, MySQL, PostgreSQL, Redis, Memcached, Node, and all of the other software you need to develop amazing Laravel applications.

Homestead provides a consistent development experience for all team members, since it is pre-configured to give everyone on your team the ability to work with the same setup.

Checkout the [official documentation on Laravel for information on Installing and configuring Homestead](https://laravel.com/docs/11.x/homestead).

### So, Which Option?

In my opinion, Herd is the fastest way to get started. Use Sail if Docker is needed. Use Homestead for advanced team experience.

That's it.

What option are you considering? If you use a different setup, which tools work for you?

Let me know on X [@mwangikanothe](https://x.com/mwangikanothe).

Cheers.
