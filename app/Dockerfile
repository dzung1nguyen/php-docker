#
#--------------------------------------------------------------------------
# Image Setup
#--------------------------------------------------------------------------
#

FROM phusion/baseimage:0.11

RUN DEBIAN_FRONTEND=noninteractive
RUN locale-gen en_US.UTF-8

ENV LANGUAGE=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
ENV LC_CTYPE=en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV TERM xterm


# Add a non-root user to prevent files being created with root permissions on host machine.
# Add a non-root user to prevent files being created with root permissions on host machine.
ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

# always run apt update when start and after add new source list, then clean up at end.
RUN set -xe; \
    apt-get update -yqq && \
#   pecl channel-update pecl.php.net && \
    groupadd -g ${PGID} xxx && \
    useradd -u ${PUID} -g xxx -m xxx -G docker_env && \
    usermod -p "*" xxx -s /bin/bash && \
    apt-get install -yqq \
      apt-utils \
      #
      #--------------------------------------------------------------------------
      # Mandatory Software's Installation
      #--------------------------------------------------------------------------
      #
      # Mandatory Software's such as ("php-cli", "git", "vim", ....) are
      # installed on the base image 'laradock/workspace' image. If you want
      # to add more Software's or remove existing one, you need to edit the
      # base image (https://github.com/Laradock/workspace).
      #
      # next lines are here because there is no auto build on dockerhub see https://github.com/laradock/laradock/pull/1903#issuecomment-463142846
      libzip-dev zip unzip \
      # Install the zip extension
      # nasm
      nasm



# Add the "PHP 7" ppa
 RUN apt-get install -y software-properties-common && \
    add-apt-repository -y ppa:ondrej/php
#
#--------------------------------------------------------------------------
# Software's Installation
#--------------------------------------------------------------------------
#
RUN echo 'DPkg::options { "--force-confdef"; };' >> /etc/apt/apt.conf

ARG INSTALL_PHP=false
ARG PHP_VERSION=7.4

RUN if [ ${INSTALL_PHP} = true ]; then \
    if [ ${PHP_VERSION} = "7.4" ]; then \
        apt-get update && \
        apt-get upgrade -y && \
        apt-get install -y --allow-downgrades --allow-remove-essential \
            --allow-change-held-packages \
            php7.4-cli \
            php7.4-common \
            php7.4-curl \
            php7.4-intl \
            php7.4-json \
            php7.4-xml \
            php7.4-mbstring \
            php7.4-mysql \
            php7.4-pgsql \
            php7.4-sqlite \
            php7.4-sqlite3 \
            php7.4-zip \
            php7.4-bcmath \
            php7.4-memcached \
            php7.4-gd \
            php7.4-imagick \
            php7.4-dev \
            pkg-config \
            libcurl4-openssl-dev \
            libedit-dev \
            libssl-dev \
            libxml2-dev \
            xz-utils \
            libsqlite3-dev \
            sqlite3 \
            git \
            curl \
            vim \
            nano \
            postgresql-client \
        && apt-get clean \
    ;fi \
;fi

ARG PHP_VERSION

#####################################
# Composer:
#####################################

# Install composer and add its bin to the PATH.
RUN if [ ${INSTALL_PHP} = true ]; then \
    curl -s http://getcomposer.org/installer | php && \
        echo "export PATH=${PATH}:/var/www/vendor/bin" >> ~/.bashrc && \
        mv composer.phar /usr/local/bin/composer \
;fi        

# Start as root
USER root

###########################################################################
# Node / NVM:
###########################################################################

# Check if NVM needs to be installed
ENV NVM_DIR /home/root/.nvm
ARG INSTALL_NODE=false
ARG INSTALL_NPM_GULP=false
ARG INSTALL_NPM_BOWER=false
ARG INSTALL_NPM_VUE_CLI=false
ARG INSTALL_NPM_ANGULAR_CLI=false

RUN if [ ${INSTALL_NODE} = true ]; then \
    mkdir -p $NVM_DIR && \
    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash \
    && . $NVM_DIR/nvm.sh \
    && nvm install node \
    && nvm use node \
    && nvm alias node \
    && if [ ${INSTALL_NPM_GULP} = true ]; then \
    npm install -g gulp \
    ;fi \
    && if [ ${INSTALL_NPM_BOWER} = true ]; then \
    npm install -g bower \
    ;fi \
    && if [ ${INSTALL_NPM_VUE_CLI} = true ]; then \
    npm install -g @vue/cli \
    ;fi \
    && if [ ${INSTALL_NPM_ANGULAR_CLI} = true ]; then \
    npm install -g @angular/cli \
    ;fi \
    && ln -s `npm bin --global` $NVM_DIR/.node-bin \
;fi

# Wouldn't execute when added to the RUN statement in the above block
# Source NVM when loading bash since ~/.profile isn't loaded on non-login shell
RUN if [ ${INSTALL_NODE} = true ]; then \
    echo "" >> ~/.bashrc && \
        echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.bashrc && \
        echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm' >> ~/.bashr \
;fi

RUN if [ ${INSTALL_NODE} = true ]; then \
    echo "" >> ~/.bashrc && \
        echo 'export NVM_DIR="/home/root/.nvm"' >> ~/.bashrc && \
        echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm' >> ~/.bashrc \
;fi
# Add PATH for node
ENV PATH $PATH:/home/root/.node-bin

# Make it so the node modules can be executed with 'docker-compose exec'
# We'll create symbolic links into '/usr/local/bin'.
RUN if [ ${INSTALL_NODE} = true ]; then \
    find $NVM_DIR -type f -name node -exec ln -s {} /usr/local/bin/node \; && \
        NODE_MODS_DIR="$NVM_DIR/versions/node/$(node -v)/lib/node_modules" && \
        ln -s $NODE_MODS_DIR/bower/bin/bower /usr/local/bin/bower && \
        ln -s $NODE_MODS_DIR/gulp/bin/gulp.js /usr/local/bin/gulp && \
        ln -s $NODE_MODS_DIR/npm/bin/npm-cli.js /usr/local/bin/npm && \
        ln -s $NODE_MODS_DIR/npm/bin/npx-cli.js /usr/local/bin/npx && \
        ln -s $NODE_MODS_DIR/vue-cli/bin/vue /usr/local/bin/vue && \
        ln -s $NODE_MODS_DIR/vue-cli/bin/vue-init /usr/local/bin/vue-init && \
        ln -s $NODE_MODS_DIR/vue-cli/bin/vue-list /usr/local/bin/vue-list \
;fi
EXPOSE 4200
# Set default work directory
WORKDIR /var/www/html