FROM php:7.4-fpm

# set timezone
ENV TIMEZONE=Asia/Manila

# make api directory
RUN mkdir -p /usr/share/nginx/api

# copy composer and lock file
COPY composer.lock composer.json /usr/share/nginx/api/

# set working directory
WORKDIR /usr/share/nginx/api

# install
RUN apt-get update && apt-get install -y \
    build-essential \
    vim \
    locales \
    zip \
    unzip \
    git \
    curl \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    jpegoptim optipng pngquant gifsicle

# clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# install php extensions
RUN docker-php-ext-install pdo_mysql exif pcntl
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install gd

# install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# add user
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# copy to default nginx folder
COPY . /usr/share/nginx/api

# copy application directory permission
COPY --chown=www . /usr/share/nginx/api

# change user to www
USER www

# run composer
RUN composer install

# expose port 7777
# EXPOSE 7777

CMD ["php-fpm"]