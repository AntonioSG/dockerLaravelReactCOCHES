FROM php:8.3.0-apache-bullseye


# Instalar dependencias del sistema
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    zip \
    unzip

# Limpiar caché
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Instalar extensiones de PHP
RUN docker-php-ext-install pdo_mysql zip mbstring exif pcntl bcmath gd


# Instalar Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

#Establecer el fichero php-ini a producción o desarrollo
#RUN cp $PHP_INI_DIR/php.ini-production $PHP_INI_DIR/conf.d/php.ini

RUN cp $PHP_INI_DIR/php.ini-development $PHP_INI_DIR/conf.d/php.ini


RUN sed -i 's/;extension=zip/extension=zip/g' $PHP_INI_DIR/conf.d/php.ini
RUN sed -i 's/;extension=pdo_mysql/extension=pdo_mysql/g' $PHP_INI_DIR/conf.d/php.ini
RUN sed -i 's#DocumentRoot /var/www/html#DocumentRoot /var/www/html/LaravelCochesAPI/public#g' /etc/apache2/sites-available/000-default.conf


RUN a2enmod rewrite



USER www-data
RUN git clone https://github.com/AntonioSG/LaravelCochesAPI.git
WORKDIR /var/www/html/LaravelCochesAPI

#El ficchero .env debería copiarse desde fuera del contenedor
RUN cp .env .
RUN composer install
RUN touch database/database.sqlite
RUN php artisan migrate


