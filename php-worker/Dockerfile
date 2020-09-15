ARG PHP_VERSION=7.4
FROM php:${PHP_VERSION}-fpm

# Use the default  configuration
ARG PHP_VERSION
 
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
        wget \
        curl \
        supervisor

# Download script to install PHP extensions and dependencies
ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions /usr/local/bin/

# PHP extensions https://github.com/mlocati/docker-php-extension-installer
RUN chmod uga+x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions gd imagick intl mysqli pdo_mysql xsl zip
 
COPY ./supervisord.conf /etc/supervisor
 
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]