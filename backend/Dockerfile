# syntax=docker/dockerfile:1

FROM php:8.2-fpm

RUN apt-get update && apt-get install -y \
    git zip unzip curl libzip-dev \
    && docker-php-ext-install pdo pdo_mysql zip

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# 👉 Create Laravel in subfolder to avoid conflict with build context
RUN composer create-project laravel/laravel /var/www/laravel

WORKDIR /var/www/laravel

RUN chown -R www-data:www-data /var/www/laravel && chmod -R 755 /var/www/laravel

EXPOSE 8000

CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=8000"]
