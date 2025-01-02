# Use the official PHP image as the base
FROM php:8.0-fpm

# Install system dependencies
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    git \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd

# Set working directory
WORKDIR /var/www

# Copy the composer.json and composer.lock
COPY composer.json composer.lock /var/www/

# Install PHP dependencies via Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer install

# Copy the rest of the application files
COPY . /var/www

# Set file permissions
RUN chown -R www-data:www-data /var/www

# Expose the port Laravel uses
EXPOSE 9000

# Start PHP-FPM server
CMD ["php-fpm"]
