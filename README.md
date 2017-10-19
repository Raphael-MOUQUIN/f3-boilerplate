# f3-boilerplate

**NOTE:** The branch skeleton-nodb-nobs has NO DATABASE features enabled by default and NO BOOTSTRAP so you are free to choose as you wish (will ened to uncomment sections of files which reference 'db' to re-enable db).

[![Travis CI](https://travis-ci.org/vijinho/f3-boilerplate.svg?branch=dev-master)](https://travis-ci.org/vijinho/f3-boilerplate)
[![Build Status](https://scrutinizer-ci.com/g/vijinho/f3-boilerplate/badges/build.png?b=dev-master)](https://scrutinizer-ci.com/g/vijinho/f3-boilerplate/build-status/dev-master)
[![Code Coverage](https://scrutinizer-ci.com/g/vijinho/f3-boilerplate/badges/coverage.png?b=dev-master)](https://scrutinizer-ci.com/g/vijinho/f3-boilerplate/?branch=dev-master)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/vijinho/f3-boilerplate/badges/quality-score.png?b=dev-master)](https://scrutinizer-ci.com/g/vijinho/f3-boilerplate/?branch=dev-master)

![f3-boilerplate homepage screenshot](tmp/screenshot.png)

Skeleton PHP7 Fatfree-framework MVC website codebase.

**Project Goal:** Be a good and extremely flexible starting-point for implementing any kind of PHP project in F3.

## Usage

- Clone the project
- Change the `app/config/default.ini` and `config.example.ini` files to suit
  your project - create your local setup `config.ini` file from this.
- Change the files in `app/lib/App` and the associated routes in `app/config/routes-*.ini` to suit.
- Run `composer dump` to update your the autoloader with your newly created classes

*PHP7 only now!* - see [CHANGELOG.md](CHANGELOG.md)

## Is this project for you?

I wrote this project for myself, but if you are thinking to use it, thinking on the points below will help you decide.

* You want to implement a website using the Fat-Free Framework
* You need a stable project that can be easily adapted and altered to suit whatever your web development needs are.
* You need to quickly and easily integrate composer classes into a project structure to get up and running ASAP.
* You need to write some boilerplate code to add project structure and initialise some commons tasks like config, logging, database connections, set up environments for production and development etc
* You may want the ability to setup your database connections in the http format - dbms://user:host@server:port/databasename
* You are thinking to run f3 on the command-line and want to see how it could be done.
* You are thinking to write an API based on REST responses and would like a starting point for to how to implement it in f3.
* You would like to see a real-life example of f3 features for render markdown, display geo-location, database connectivity.
* You want to have your project configuration split up into different files for the main configuration as have a local override file.
* You would like to have your script log how long it took to run and how much memory it used after executing when in 'development' mode.
* You need to make sure that ALL script input is normalised and cleaned by default.
* You want to use namespaces in your project

## Setup

### Composer and Webserver

- [Get Composer](https://getcomposer.org/) - `curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin -filename=composer`
- Run `composer update`
- Setup webserver config from [app/config/webserver](app/config/webserver)
- OR run with php in-built webserver from [www](www): `php -S http://127.0.0.1:8080` and browse to [http://127.0.0.1:8080](http://127.0.0.1:8080)

### Configuration
  - Copy `app/config/config.example.ini` to `config.ini`
  - Edit `app/config/config.ini `and add anything extra from `default.ini` for overrides
  - In the top level folder `run composer install`

### Database
  - Create a database dumpfile to auto-import in `data/db/sql/create.sql` (set ini option for db.create=true)

### Folders & Permissions
Setup empty website folders as follows:

```
mkdir -p tmp/cache tmp/sessions tmp/uploads tmp/logs data
sudo chown -fR www-data:www-data tmp data
sudo chmod -fR 777 tmp data
```

## Description of Project Layout

*Note:* The files that were in `lib/FFMVC` have now been split-out into their own repository https://github.com/vijinho/FFMVC
They can then be included in your own project by adding the same lines in your `composer.json` as used in mine here.

## Core Files/Paths

 *  `lib/bcosca/fatfree-core` - [fatfree framework (core)](https://github.com/bcosca/fatfree-core) lives here
 * `www` - website and public doc root (aka `public_html` or `htdocs` etc)
 * `www/index.php` - start website application here - is the default file used by `.htaccess` for routing
 * `app` - the website application lives outside the webroot for security
 * `app/lib/App/App.php` - start fatfree project by including this file and executing Run();
 * `lib/` - all external library files/classes
 * `app/lib` - local application-specific libraries
 * `app/lib/App/Controllers` - MVC Controllers
 * `app/lib/App/Controllers/API` - MVC Rest API Controllers
 * `app/lib/App/CLI` - Controllers for when executing in a command-line environemnt
 * `tmp/cache` `tmp/sessions` `tmp/uploads` - temporary files
 * `tmp/logs` - application logfiles
 * `data` - website data storage folder
 * `docs` - application documentation (markdown files)
 * `app/config` - application configuration files
 * `app/config/vhost` - application virtual host configuration files (apache and nginx supported)
 * `bin/cli.php` - symlink to command-line runner in `app/lib/App/CLI.php' which uses routes in `app/config/routes-cli.ini`
 * `app/en/templates/error/` - these files are standard php includes, not f3 templates, used by the error handler function
 * `app/en/templates/error/debug.phtml` - debug error page (if DEBUG=3)
 * `app/en/templates/error/404.phtml` - 'friendly' file not found page
 * `app/en/templates/error/error.phtml` - 'friendly' error page
 * `tests` - unit tests see [docs/TESTING.md](docs/TESTING.md])

## Supplemental Files/Paths

Used for bootstrapping the application and generic enough to be separate from the main project.

 * `lib/FFMVC/App.php` - Base Application Class to start/shutdown app
 * `lib/FFMVC/App/Helpers` - Auxillary helper functions and utility libraries specific to the project

### External Libraries

 * [DICE](https://github.com/Level-2/Dice) dependency injection container
 * [Whoops](https://github.com/filp/whoops) for nicer error handling if level of DEBUG=4
 * [Climate](http://climate.thephpleague.com/) is used for the CLI utility methods.

#### SSL (Optional)

Test certificates in [app/config/webserver/ssl/](app/config/webserver/ssl/)
Use [Let's Encrypt](https://letsencrypt.org) to generate a real live valid SSL certificate for production environments.

#### Dummy certificate (example)

`openssl req -new -newkey rsa:4096 -days 365 -nodes -x509 -subj "/C=GB/ST=STATE/L=TOWN/O=Office/CN=f3-boilerplate.local" -keyout f3-boilerplate.key -out f3-boilerplate.crt`

Add to apache virtual host (and also see the f3-boilerplate-ssl.local files in [app/config/webserver/](app/config/webserver/)

```
    SSLCertificateFile ssl/f3-boilerplate.crt
    SSLCertificateKeyFile ssl/f3-boilerplate.key
```
[MAMP](https://www.mamp.info/) lets you add the SSL file in the Hosts/SSL tab.

## Running from the CLI

From the base folder run:

```
php app/lib/App/CLI.php /
```

The output will be:

```
CLI Script
FFCMS\CLI\Index::index
Hello World!
Finished.
Script executed in 0.048 seconds.
Memory used 2.29 MB.
```

--
http://www.urunu.com
