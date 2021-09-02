FROM php:7.4-fpm
ARG TIMEZONE
ARG HOST_IP

RUN apt-get update && apt-get install -y git unzip openssl procps acl zlib1g-dev libzip-dev zip

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Configure git
RUN git config --global user.email "eng.mohamed.dahab@gmail.com" && git config --global user.name "Mohamed Dahab"

# Install symfony console
RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony/bin/symfony /usr/local/bin/symfony

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini

# Install extensions
RUN docker-php-ext-install pdo pdo_mysql bcmath zip

# Install xdebug
RUN pecl install xdebug-2.9.1 && docker-php-ext-enable xdebug

# Add custom config
COPY config/custom.ini $PHP_INI_DIR/conf.d/zzz_custom.ini

WORKDIR /var/www/html/demo