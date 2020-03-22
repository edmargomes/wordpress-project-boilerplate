# Composer Template for WordPress Projects using Docker

### Dependencies
* Docker
* Docker Compose
* Make

### How to install

1. Rename `.env.example` to `.env` and change values.

2. Run `make up`

3. If your first run then you need access the container and run composer install
```shell script
make shell
composer install
```

4. Add your PROJECT_BASE_URL in your /etc/hosts. 
Example:
```shell script
127.0.0.1       wp.docker.local
```

5. Access PROJECT_BASE_URL using port 8000 in your browser. Example: wp.docker.local:8000

**Enjoy!!!**

**OBS.:** Look docker.mk to see others commands

### Description

This project template should provide a kickstart for managing your site dependencies with [Composer](https://getcomposer.org/)
and Docker.

This project consist of:

* WordPress core: [wodby/wordpress-composer](https://github.com/wodby/wordpress-composer)
* Repository https://wpackagist.org/ to install WordPress plugins and themes
* `composer/installers` to set custom paths for plugins and themes
* `drupal-composer/preserve-paths` to exclude paths for plugins and themes under version control 
* Docker4Wordpress: [wodby/docker4wordpress](https://github.com/wodby/docker4wordpress)

Current WordPress core: `~5.0`

### Introduction about Docker4Wordpress

Docker4WordPress is a set of docker images optimized for WordPress. 
Use docker-compose.yml file from this repository to spin up a local environment for WordPress on Linux, macOS and Windows. 

* Read the docs on [**how to use**](https://wodby.com/docs/stacks/wordpress/local#usage)
* Ask questions on [Slack](http://slack.wodby.com/)
* Follow [@wodbycloud](https://twitter.com/wodbycloud) for future announcements

### Stack

The WordPress stack consist of the following containers:

| Container       | Versions                | Service name    | Image                              | Default |
| -------------   | ----------------------- | ------------    | ---------------------------------- | ------- |
| [Nginx]         | 1.17, 1.16              | `nginx`         | [wodby/nginx]                      | ✓       |
| [Apache]        | 2.4                     | `apache`        | [wodby/apache]                     |         |
| [WordPress]     | 5                       | `php`           | [wodby/wordpress]                  | ✓       |
| [PHP]           | 7.4, 7.3, 7.2           | `php`           | [wodby/wordpress-php]              |         |
| [MariaDB]       | 10.4, 10.3, 10.2, 10.1  | `mariadb`       | [wodby/mariadb]                    | ✓       |
| [PostgreSQL]    | 12, 11, 10, 9.x         | `postgres`      | [wodby/postgres]                   |         |
| [Redis]         | 5, 4                    | `redis`         | [wodby/redis]                      |         |
| [Memcached]     | 1                       | `memcached`     | [wodby/memcached]                  |         |
| [Varnish]       | 6.0, 4.1                | `varnish`       | [wodby/varnish]                    |         |
| [Node.js]       | 12, 10, 8               | `node`          | [wodby/node]                       |         |
| [Solr]          | 8, 7, 6, 5              | `solr`          | [wodby/solr]                       |         |
| [Elasticsearch] | 7, 6                    | `elasticsearch` | [wodby/elasticsearch]              |         |
| [Kibana]        | 7, 6                    | `kibana`        | [wodby/kibana]                     |         |
| [AthenaPDF]     | 2.10.0                  | `athenapdf`     | [arachnysdocker/athenapdf-service] |         |
| [Mailhog]       | latest                  | `mailhog`       | [mailhog/mailhog]                  | ✓       |
| [OpenSMTPD]     | 6.0                     | `opensmtpd`     | [wodby/opensmtpd]                  |         |
| [Rsyslog]       | latest                  | `rsyslog`       | [wodby/rsyslog]                    |         |
| [Blackfire]     | latest                  | `blackfire`     | [blackfire/blackfire]              |         |
| [Webgrind]      | 1                       | `webgrind`      | [wodby/webgrind]                   |         |
| [XHProf viewer] | latest                  | `xhprof`        | [wodby/xhprof]                     |         |
| Adminer         | 4.6                     | `pma`           | [wodby/adminer]                    |         |
| phpMyAdmin      | latest                  | `pma`           | [phpmyadmin/phpmyadmin]            |         |
| Portainer       | latest                  | `portainer`     | [portainer/portainer]              | ✓       |
| Traefik         | latest                  | `traefik`       | [_/traefik]                        | ✓       |

### Documentation

Full documentation is available at https://wodby.com/docs/stacks/wordpress/local.

### Images' tags

Images tags format is `[VERSION]-[STABILITY_TAG]` where:

`[VERSION]` is the _version of an application_ (without patch version) running in a container, e.g. `wodby/nginx:1.15-x.x.x` where Nginx version is `1.15` and `x.x.x` is a stability tag. For some images we include both major and minor version like PHP `7.2`, for others we include only major like Redis `5`. 

`[STABILITY_TAG]` is the _version of an image_ that corresponds to a git tag of the image repository, e.g. `wodby/mariadb:10.2-3.3.8` has MariaDB `10.2` and stability tag [`3.3.8`](https://github.com/wodby/mariadb/releases/tag/3.3.8). New stability tags include patch updates for applications and image's fixes/improvements (new env vars, orchestration actions fixes, etc). Stability tag changes described in the corresponding a git tag description. Stability tags follow [semantic versioning](https://semver.org/).

We highly encourage to use images only with stability tags.

### Beyond local environment

Docker4WordPress is a project designed to help you spin up local environment with docker-compose. If you want to deploy a consistent stack with orchestrations to your own server, check out [![WordPress stack on Wodby](https://www.google.com/s2/favicons?domain=wodby.com) Wodby](https://wodby.com/stacks/wordpress).


### Paths

By default, wordpress core will be installed in `./web` directory. Plugins and themes will be installed in `./web/wp-content/plugins` and `./web/wp-content/themes`. Point your Apache vhost or similar to this project's `./web` directory.

### Usage


### How to install WordPress plugins and themes?

With `composer require ...` you can download new dependencies to your installation.

```
cd some-dir
composer require wpackagist-plugin/wp-cfm
```

### How to manage my custom themes and plugins under version control?

1. Exclude path to your plugin or theme from .gitignore. Example for theme under `web/wp-content/themes/my-custom-theme/`:
    ```
    !web/
    web/*
    !web/wp-content/
    web/wp-content/*
    !web/wp-content/themes/
    web/wp-content/themes/*
    !web/wp-content/themes/my-custom-theme/
    ``` 
2. Add the same path to your composer.json under `extra > preserve-paths`: 
    ```
    "preserve-paths": [
      "web/wp-content/themes/custom"
    ]
    ```
3. Add your plugin/theme directory under version control
4. Run `composer install`. Composer will install WordPress core and keep your custom theme
