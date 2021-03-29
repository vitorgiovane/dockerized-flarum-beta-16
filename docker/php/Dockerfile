FROM php:7.4-fpm

# Set working directory
WORKDIR /atmosphere

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    libzip-dev \
    jpegoptim optipng pngquant gifsicle \
    nano \
    unzip \
    git \
    libpq-dev \
    libonig-dev \
    wget \
    curl

RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install extensions
RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl

# Install composer
RUN curl https://getcomposer.org/download/2.0.8/composer.phar --output /usr/local/bin/composer && \
    chmod +x /usr/local/bin/composer

# Add user for application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Copy existing application directory contents
COPY . /atmosphere

# Copy existing application directory permissions
COPY --chown=www:www . /atmosphere

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
