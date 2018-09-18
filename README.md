# Nginx, php-fpm and runit base image

## Description

Out of the box, this image offers a working nginx and php-fpm webserver.
This image is intended to allow to run a PHP application either passing
a PHP application folder webroot as argument, either writing a Dockerfile
with `nasqueron/nginx-php7-fpm` image as base image.

Your web directory — if you don't add vhosts — is
`/var/wwwroot/default` (mounted as volume).

The PHP last 7.1 version is compiled through a build process borrowed from
the official PHP Docker image, with [this Dockerfile used](https://github.com/docker-library/php/blob/08bf31dfd492f02a2696c9a30eb85326b1570abd/5.6/fpm/Dockerfile).

We add common extensions like calendar, curl, gd, iconv, libxml, mbstring,
mysqli, PDO MySQL and pcntl. The Pear, PECL executables and utilities
(including build stuff like phpize) are available too.

Once running, you can quickly add PHP extensions to this image,
with `docker-php-ext-configure` and `docker-php-ext-install` scripts.

Nginx is installed through the [nginx-full Debian package](https://wiki.debian.org/Nginx).
SSL is ready if needed at the container level (we expose ports 80 and 443).

Services are managed by [runit](http://smarden.org/runit/) in `/etc/service` directory.

## How to use it

To rebuild this image:

    docker build --tag nasqueron/nginx-php7-fpm .

To rebuild a fork of this image based on a modified Dockerfile:

    docker build --tag your-image-name-tag .

To launch a container to execute a PHP application in /data/awesome-php-app
with http://localhost:8080 as address:

    docker run -d -v /data/awesome-php-app:/var/wwwroot/default -p 8080:80 nasqueron/nginx-php7-fpm

To create an image for an application with thisas base, create a Dockerfile:

    FROM nasqueron/docker-nginx-php7-fpm
    # Debian commands to deploy your application code
    # If you need other processes, adds a /etc/service/<service name>/run file

That's it.

## How to upgrade this image?

As noted in https://devcentral.nasqueron.org/T787 we need to sync files and novolume/files.

For that, you can use our helper Makefile:
```
cd novolume
make update
```
