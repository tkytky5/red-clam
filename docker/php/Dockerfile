FROM php:8.3-fpm
RUN apt-get update\
  && apt install -y\
  sudo\
  curl\
  git\
  zip\
  vim\
  npm

# composer install
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === 'c8b085408188070d5f52bcfe4ecfbee5f727afa458b2573b8eaaf77b3419b0bf2768dc67c86944da1544f06fa544fd47') { echo 'Installer verified'.PHP_EOL; } else { echo 'Installer corrupt'.PHP_EOL; unlink('composer-setup.php'); exit(1); }"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN sudo mv composer.phar /usr/local/bin/composer

RUN docker-php-ext-install pdo_mysql
RUN composer global require laravel/installer
WORKDIR /var/www/html
RUN composer create-project --prefer-dist laravel/laravel red-clam

ADD . /var/www/html/storage
RUN chown -R www-data:www-data /var/www/html/red-clam/storage
RUN composer install
RUN npm install