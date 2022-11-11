FROM php:8.1-fpm-alpine

# Download script to install PHP extensions and dependencies
ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions /usr/local/bin/

# PHP extensions https://github.com/mlocati/docker-php-extension-installer
RUN chmod uga+x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions gd imagick intl mysqli pdo_mysql xsl zip exif


# Configure non-root user.
# www-data nginx user
ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

RUN apk add shadow && usermod -u ${PUID} www-data && groupmod -g ${PGID} www-data

# Install composer
RUN cd /var/tmp/ && curl -s http://getcomposer.org/installer | php && mv ./composer.phar /usr/local/bin/composer

USER www-data