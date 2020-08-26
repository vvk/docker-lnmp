FROM php:7.4-fpm
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
    && docker-php-ext-install -j$(nproc) \
        gettext mysqli pdo_mysql zip \
        bcmath opcache sockets soap \
    && docker-php-ext-configure gd  --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd

CMD ["php-fpm", "-F"]
