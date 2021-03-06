#
# Use this dockerfile to run the application.
#
# Start the server using docker-compose:
#
#   docker-compose build
#   docker-compose up
#
# You can install dependencies via the container:
#
#   docker-compose run readbooks composer install
#
# You can manipulate dev mode from the container:
#
#   docker-compose run readbooks composer development-enable
#   docker-compose run readbooks composer development-disable
#   docker-compose run readbooks composer development-status
#
# OR use plain old docker 
#
#   docker build -f Dockerfile-dev -t readbooks .
#   docker run -it -p "8080:80" -v $PWD:/var/www readbooks
#
FROM php:7.2-apache

RUN apt-get update \
 && apt-get install -y git zlib1g-dev mysql-client \
 && docker-php-ext-install zip mysqli pdo_mysql \
 && pecl install xdebug \
 && docker-php-ext-enable xdebug \
 && echo 'zend_extension="/usr/local/lib/php/extensions/no-debug-non-zts-20170718/xdebug.so"' >> /usr/local/etc/php/php.ini \
 && echo 'xdebug.remote_enable=1' >> /usr/local/etc/php/php.ini \
 && echo 'xdebug.remote_connect_back=1' >> /usr/local/etc/php/php.ini \
 && a2enmod rewrite \
 && sed -i 's!/var/www/html!/var/www/public!g' /etc/apache2/sites-available/000-default.conf \
 && mv /var/www/html /var/www/public \
 && curl -sS https://getcomposer.org/installer \
  | php -- --install-dir=/usr/local/bin --filename=composer \
 && echo "AllowEncodedSlashes On" >> /etc/apache2/apache2.conf

WORKDIR /var/www
