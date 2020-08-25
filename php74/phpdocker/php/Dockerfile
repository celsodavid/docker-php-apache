FROM php:7.4-apache

WORKDIR "/application"

# Fix defined timezone
ENV TZ=America/Sao_Paulo
RUN echo $TZ > /etc/timezone

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

#download composer
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer

RUN apt-get update && apt-get install --no-install-recommends -y && apt-get install -y wget \
    unzip \
    lsb-release \
    apt-transport-https \
    libxslt1-dev \
    zlib1g-dev \
    libpng-dev \
    libjpeg-dev \
    build-essential \
    libaio-dev && \
    apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install intl gettext
RUN docker-php-ext-enable intl gettext

#RUN docker-php-ext-install xsl zip xmlrpc gd && \
#    docker-php-ext-enable xsl zip xmlrpc gd

#Copy apache2.conf customized
#COPY phpdocker/apache/apache.conf /etc/apache2/sites-enabled/

#Enable rewrite/ssl module apache
RUN a2enmod rewrite

#COPY phpdocker/php/php-ini-overrides.ini /usr/local/etc/php/php.ini

COPY . /application

#install and configure xdebug
RUN yes | pecl install xdebug
RUN echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" >> /usr/local/etc/php/php.ini
RUN echo 'xdebug.remote_port=9000' >> /usr/local/etc/php/php.ini
RUN echo 'xdebug.remote_enable=1' >> /usr/local/etc/php/php.ini
RUN echo 'xdebug.remote_connect_back=1' >> /usr/local/etc/php/php.ini
RUN echo 'xdebug.overload_var_dump=1' >> /usr/local/etc/php/php.ini
RUN echo 'xdebug.cli_color=1' >> /usr/local/etc/php/php.ini
RUN echo 'xdebug.idekey=PHPSTORM' >> /usr/local/etc/php/php.ini

#install composer dependencies
#RUN cd /var/www/html && composer install --optimize-autoloader --no-scripts --no-plugins --no-dev --prefer-dist

EXPOSE 80