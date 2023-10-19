FROM php:8.2-fpm

ENV APP_PATH=/var/www/html

WORKDIR ${APP_PATH}

USER root

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

# Install dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    build-essential \
    libpng-dev \
    libwebp-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    nano \
    sudo \
    git \
    curl \
    libicu-dev \
    zlib1g-dev \
    libxml2 \
    libxml2-dev \
    libzip-dev \
    libreadline-dev \
    libonig-dev \
    potrace \
    imagemagick \
    libmagickwand-dev

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install extensions
RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl bcmath intl
# Enable php extensions
RUN docker-php-ext-enable pdo_mysql mbstring zip

# Install gd
RUN docker-php-ext-configure gd --with-jpeg --with-freetype --with-webp
RUN docker-php-ext-install -j$(nproc) gd
RUN docker-php-ext-enable gd

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Remove Cache
RUN rm -rf /var/cache/apk/*

# Expose port 9000 and start php-fpm server
EXPOSE 9000

CMD ["php-fpm"]
