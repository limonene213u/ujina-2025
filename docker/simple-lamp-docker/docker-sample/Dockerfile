# PHP 8.4とApacheのDockerfile
FROM php:8.4-apache

# 必要なPHP拡張をインストール
RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libzip-dev \
    libonig-dev \
    libxml2-dev \
    unzip \
    git \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) \
        gd \
        mysqli \
        pdo \
        pdo_mysql \
        zip \
        mbstring \
        xml \
        opcache

# Apacheのmod_rewriteを有効化
RUN a2enmod rewrite

# Composerをインストール
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# 作業ディレクトリを設定
WORKDIR /var/www/html

# www-dataユーザーに適切な権限を設定
RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 755 /var/www/html

# Apacheを起動
EXPOSE 80
CMD ["apache2-foreground"]