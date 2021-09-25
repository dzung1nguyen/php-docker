FROM php:7.4-fpm

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
        wget \
        curl \
        supervisor

# Download script to install PHP extensions and dependencies
ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions /usr/local/bin/

# PHP extensions https://github.com/mlocati/docker-php-extension-installer
RUN chmod uga+x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions gd imagick intl mysqli pdo_mysql xsl zip exif
 
COPY ./supervisord.conf /etc/supervisor
 
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]

EXPOSE 9009