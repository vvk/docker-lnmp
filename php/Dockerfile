FROM php:8.2-fpm
RUN apt-get update && apt-get install -y \
        zlib1g-dev libzip-dev \ 
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libmemcached-dev \
        zlib1g-dev \
        libcurl4-openssl-dev \
        libxml2-dev \
        --no-install-recommends && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-configure gd  --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) gettext calendar pdo_mysql bcmath zip exif ftp \
        intl mysqli opcache sockets soap \
    && pecl install mongodb csv swoole tideways xhprof xlswriter xdebug mysqli redis \

    # 安装 Composer
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
    # 设置 Composer 镜像源，加快 Composer 依赖包的下载速度 laravel11会有问题
    # && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

WORKDIR /var/www/html

CMD ["php-fpm", "-F"]
