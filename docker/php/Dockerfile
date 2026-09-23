FROM php:8.4-fpm-bookworm

RUN apt-get update \ 
    && apt-get install -y \ 
    git \ 
    unzip \ 
    libpq-dev \ 
    libicu-dev \ 
    && docker-php-ext-install \ 
    pdo_pgsql \ 
    intl \ 
    opcache \ 
    && rm -rf /var/lib/apt/lists/* 

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer 

WORKDIR /app 

COPY composer.json composer.lock ./ 

RUN composer install \ 
    --no-dev \ 
    --no-interaction \ 
    --no-progress \ 
    --prefer-dist \ 
    --optimize-autoloader \ 
    --no-scripts 

COPY . . 

RUN composer run-script post-install-cmd 2>/dev/null || true 

RUN mkdir -p runtime web/assets \ 
    && chown -R www-data:www-data runtime web/assets 

USER www-data 

CMD ["php-fpm"]