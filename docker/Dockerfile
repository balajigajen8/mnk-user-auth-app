# Use the official PHP image as the base
FROM php:8.0-fpm as builder

# Install system dependencies and PHP extensions
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    git \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Set working directory
WORKDIR /var/www

# Copy composer.json and composer.lock to utilize Docker layer caching
COPY composer.json composer.lock /var/www/

# Install PHP dependencies via Composer (use flags to optimize)
RUN composer install --no-interaction --prefer-dist --optimize-autoloader

# Copy the rest of the application files into the container
COPY . /var/www

# Set appropriate file permissions for Laravel (necessary for storage and cache)
RUN chown -R www-data:www-data /var/www

# Final stage: Create a smaller image for serving the application
FROM php:8.0-fpm

# Install necessary dependencies for runtime
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    git \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd

# Copy the application files and installed dependencies from the builder stage
COPY --from=builder /var/www /var/www

# Set working directory
WORKDIR /var/www

# Expose the port for PHP-FPM
EXPOSE 9000

# Start PHP-FPM server
CMD ["php-fpm"]
