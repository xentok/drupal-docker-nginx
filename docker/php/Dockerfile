FROM php:7.2-fpm

# Install extensions
RUN apt-get update \
	&& apt-get install -y libicu-dev zip unzip curl zlib1g-dev libpng-dev vim procps git\
	&& docker-php-ext-install intl opcache \
	&& docker-php-ext-enable intl opcache \
	&& docker-php-ext-install pdo pdo_mysql mysqli\
	&& docker-php-ext-install mbstring \
	&& pecl install xdebug-2.7.1 \
	&& docker-php-ext-enable xdebug \
	&& docker-php-ext-install gd \
	&& apt-get clean && rm -rf /var/lib/apt/lists/*

# using variables
ARG www_user
ARG www_password
ARG NODE_VERSION

ENV NVM_DIR $NVM_DIR

# add user
RUN useradd -ms /bin/bash -g www-data $www_user \
	&& echo "${www_user}:${www_password}" | chpasswd

# Install composer
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

RUN chown -R $www_user:www-data /var/www/ \
	&& chown -R $www_user:www-data /var/run/ \
	&& chown -R $www_user:www-data /var/log/

USER ${www_user}
WORKDIR /var/www/app

# install Nodejs
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
RUN export NVM_DIR=$HOME/.nvm && . $HOME/.nvm/nvm.sh \
	&& nvm install $NODE_VERSION \
	&& nvm use $NODE_VERSION

COPY ./.bashrc ~/.bashrc