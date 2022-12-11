# WP Sail

WP Sail provides a Docker powered local development experience that is compatible with macOS, Windows (WSL2), and Linux.

Based on [Laravel Sail](https://github.com/laravel/sail/) and [wordpress-sail](https://github.com/sterner-stuff/wordpress-sail/).

## Inspiration

Laravel Sail is inspired by and derived from [Vessel](https://github.com/shipping-docker/vessel) by [Chris Fidao](https://github.com/fideloper). If you're looking for a thorough introduction to Docker, check out Chris' course: [Shipping Docker](https://serversforhackers.com/shipping-docker).

In addition to modifications already made by [Ethan Clevenger](https://github.com/ethanclevenger91) I just want to have a basis to use patchfiles for updates. Also im looking into implementing Devcontainer.

## Sail customization

#### Add Wordpress command line interface for Wordpress to Container

[WP-CLI](https://wp-cli.org/de/#installation) needs to be added as global binary. 

```
RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x wp-cli.phar \
    && mv wp-cli.phar /usr/local/bin/wp
```

#### Use PHP's built-in web server serving from WP-CLI

To use the built-in web server from WP-CLI change the supervisord configuration.

```
command=/usr/bin/php -d variables_order=EGPCS /usr/local/bin/wp server --docroot=/var/www/html/web/ --host=0.0.0.0 --port=80
```

#### Composer dependencies

Remove unnecessary dependency `illuminate/console`. Add `wp-cli/wp-cli` as future console.

#### Installation Routine

Make the installation routine in src/Console compatible with WP-CLI. Also add those commands to WP-CLI via composers autoloader.

## How to use

```
composer create-project roots/bedrock wp-project
composer require hoepfner-digital/wp-sail
```

Make sure the local autoload is required as part of the WP-CLI lifecycle

```
# in wp-cli.yml
require:
    - vendor/autoload.php
```


## License

Laravel Sail is open-sourced software licensed under the [MIT license](LICENSE.md).