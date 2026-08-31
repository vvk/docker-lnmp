FROM php:8.4.25-fpm

# 安装系统依赖（必须加 libhiredis-dev + git，否则redis扩展编译失败）
RUN apt-get update && apt-get install -y \
        libfreetype-dev \
        zlib1g-dev \
        libzip-dev \
        llibfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libmcrypt-dev \
        libmemcached-dev \
        libhiredis-dev \
        libxml2-dev \
        --no-install-recommends && rm -rf /var/lib/apt/ \
        git \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) gettext calendar pdo pdo_mysql bcmath zip exif ftp \
        intl mysqli opcache sockets soap \
    && pecl install mongodb csv swoole tideways xhprof xlswriter xdebug mysqli redis fileinfo zlib \

# 安装 Redis、xlswriter 扩展 + Composer
RUN pecl install redis \
    && pecl install xlswriter-3.0.0 \
    && docker-php-ext-enable redis xlswriter \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# 清理缓存
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html

CMD ["php-fpm", "-F"]