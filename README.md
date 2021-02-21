## Installation as docker container

1) The components are 3 docker containers, 
 - app is based on php image and node will be installed here
 - mysql will be serving through custom port 3307 
 - nginx will be serving through custom port 81

2) Containers
 - Install composer and node on the image and run composer install and npm install & run dev for building production files
   
```angular2html
FROM php:7.4-fpm

# Copy composer.lock and composer.json
COPY composer.lock composer.json /var/www/

# Set working directory
WORKDIR /var/www

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    libonig-dev \
    libzip-dev \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install extensions
RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl
#RUN docker-php-ext-configure gd --with-gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install gd

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
# Install node12
RUN rm -rf /var/lib/apt/lists/ && curl -sL https://deb.nodesource.com/setup_12.x | bash -
RUN apt-get install nodejs -y

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www


# Copy existing application directory contents
COPY . /var/www
RUN cd /var/www

RUN composer install
RUN npm install
RUN npm run dev

# Copy existing application directory permissions
COPY --chown=www:www . /var/www
RUN chown -R www:www /var/www

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
```
 - Set config of nginx in /deploy/nginx/conf.d
 - Set mysql config in /deploy/mysql

``` bash

version: '3'
services:

  #PHP Service
  app:
    build:
      context: .
      dockerfile: Dockerfile
    image: digitalocean.com/php
    container_name: app
    restart: unless-stopped
    tty: true
    environment:
      SERVICE_NAME: app
      SERVICE_TAGS: dev
    working_dir: /var/www
    volumes:
      - ./:/var/www
      - ./deploy/php/local.ini:/usr/local/etc/php/conf.d/local.ini
    networks:
      - app-network

  #Nginx Service
  webserver:
    image: nginx:alpine
    container_name: webserver
    restart: unless-stopped
    tty: true
    ports:
      - "81:81"
      - "444:444"
    volumes:
      - ./:/var/www
      - ./deploy/nginx/conf.d/:/etc/nginx/conf.d/
    networks:
      - app-network

  #MySQL Service
  db:
    image: mysql:5.7.22
    container_name: db
    restart: unless-stopped
    tty: true
    ports:
      - "3307:3307"
    environment:
      MYSQL_DATABASE: db
      MYSQL_ROOT_PASSWORD: root
      MYSQL_TCP_PORT: 3307
      SERVICE_TAGS: dev
      SERVICE_NAME: mysql   
    volumes:
      - ./deploy/mysql/my.cnf:/etc/mysql/my.cnf
    networks:
      - app-network

#Docker Networks
networks:
  app-network:
    driver: bridge
#Volumes
volumes:
  dbdata:
    driver: local
```

3. Running step
   docker-compose exec app php artisan key:generate
   docker-compose exec app php artisan config:cache
   docker-compose exec app php artisan migrate

