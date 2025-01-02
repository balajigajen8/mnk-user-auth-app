# Use the official PHP image with PHP 8.1 (or another version as needed)
FROM php:8.1-cli

# Install dependencies required for your project
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    curl \
    libzip-dev \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-configure zip --with-libzip \
    && docker-php-ext-install zip \
    && docker-php-ext-install mysqli pdo pdo_mysql \
    && docker-php-ext-enable mysqli pdo pdo_mysql

# Install Composer globally
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Set the working directory in the container
WORKDIR /var/www/html

# Copy your application files into the container
COPY . /var/www/html

# Install the PHP dependencies via Composer
RUN composer install --no-interaction --prefer-dist --optimize-autoloader

# Expose port (if you're running a web server or any other service inside the container)
EXPOSE 80

# Command to run the application (if it's a web server, like Apache or Nginx, you'd replace this with the appropriate start command)
CMD ["php", "-S", "0.0.0.0:80", "-t", "/var/www/html/public"]
