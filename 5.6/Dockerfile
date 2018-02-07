FROM php:5.6-apache
MAINTAINER Volodymyr Dubyna <vladimir.dubyna@gmail.com>

RUN apt-get update

RUN build_packages=" \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libmcrypt-dev \
    libxml2-dev \
    libxslt1-dev \
    sendmail-bin \
    sendmail \
    libicu-dev" \
    && apt-get install -y $build_packages \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install mcrypt \
    && docker-php-ext-install pcntl \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install soap \
    && yes | pecl install xdebug-2.5.5 && docker-php-ext-enable xdebug \
    && docker-php-ext-install xsl \
    && docker-php-ext-install zip \
    && docker-php-ext-install intl \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV ION_CUBE_PHP_VERSION "5.6"
RUN PHP_EXTENSION_DIR="$(php-config --extension-dir)" bash -c 'curl http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz -o /ioncube_loaders_lin_x86-64.tar.gz && \
    tar -xzvf /ioncube_loaders_lin_x86-64.tar.gz -C / && \
    cp "/ioncube/ioncube_loader_lin_${ION_CUBE_PHP_VERSION}.so" $PHP_EXTENSION_DIR && \
    echo "zend_extension=${PHP_EXTENSION_DIR}/ioncube_loader_lin_${ION_CUBE_PHP_VERSION}.so" > /usr/local/etc/php/conf.d/00-ioncube.ini && \
    rm -rf /ioncube /ioncube_loaders_lin_x86-64.tar.gz'

COPY ./php.ini /usr/local/etc/php/

RUN apt-get update \
    && a2enmod ssl rewrite headers \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y ssl-cert \
    && a2ensite default-ssl \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN apt-get update && apt-get install -y wget vim mc locate netcat && updatedb
RUN printf "xdebug.remote_enable=1 \n \
    xdebug.remote_handler=dbgp \n \
    xdebug.remote_mode=req \n \
    xdebug.remote_host=docker.for.mac.localhost \n \
    xdebug.remote_port=9000 \n \
    xdebug.max_nesting_level=100000 \n \
    xdebug.var_display_max_depth=6 \n \
    xdebug.var_display_max_children=256 \n \
    xdebug.var_display_max_data=2024" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

CMD ["apache2-foreground"]
