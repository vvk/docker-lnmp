FROM php:8.4.25-fpm

# 安装系统依赖（必须加 libhiredis-dev + git，否则redis扩展编译失败）
RUN apt-get update && apt-get install -y \
        --no-install-recommends  \
        libfreetype-dev \
        zlib1g-dev \
        libzip-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libmemcached-dev \
        libhiredis-dev \
        libxml2-dev \
        git \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) gettext calendar pdo pdo_mysql bcmath zip exif ftp \
        intl mysqli opcache sockets soap \
    && pecl install mongodb csv swoole-6.0.0 xdebug redis xlswriter-3.0.0 \
    && docker-php-ext-enable mongodb csv swoole xdebug redis xlswriter \
    && rm -rf /var/lib/apt/lists/*

# 安装 Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /var/www/html
CMD ["php-fpm", "-F"]