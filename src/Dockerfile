FROM php:7.3-fpm
# Copy composer.lock and composer.json
COPY composer.lock composer.json /var/www/
# Set working directory
ARG USER_ID
ARG GROUP_ID
USER root

WORKDIR /var/www
# Install dependencies
 
RUN apt-get update && apt-get install -y \
        git \
        curl \
        wget \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev zlib1g-dev libicu-dev g++ libmagickwand-dev --no-install-recommends libxml2-dev \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl \
    && docker-php-ext-install mbstring zip xml pdo_mysql \
    && pecl install imagick \
    && docker-php-ext-enable imagick \
    && pecl install xdebug-2.9.6 \
    && docker-php-ext-enable xdebug

RUN wget https://getcomposer.org/installer -O - -q \
    | php -- --install-dir=/bin --filename=composer --quiet


RUN usermod -u ${USER_ID} www-data && groupmod -g ${GROUP_ID} www-data

WORKDIR /var/www

USER "${USER_ID}:${GROUP_ID}"
# Copy existing application directory contents
COPY . /var/www
# Copy existing application directory permissions

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]