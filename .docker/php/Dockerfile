FROM php:7.4-fpm

RUN apt-get update && apt-get install -y zlib1g-dev g++ git libicu-dev zip libzip-dev zip libpng-dev \
    && docker-php-ext-install intl opcache pdo pdo_mysql \
    && pecl install apcu \
    && docker-php-ext-enable apcu \
    && docker-php-ext-configure zip \
    && docker-php-ext-install zip

WORKDIR /var/www/project

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony/bin/symfony /usr/local/bin/symfony

# https://github.com/docker-library/php/issues/75
# Install needed php extensions: ldap
RUN \
    apt-get update && \
    apt-get install libldap2-dev -y && \
    rm -rf /var/lib/apt/lists/* && \
    docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-install ldap

#Instala o GD
RUN docker-php-ext-install gd

# Instala o driver do DB2
WORKDIR /opt/ibm

ADD  v10.5fp11_linuxx64_dsdriver.tar.gz /opt/ibm/

RUN     cd /opt/ibm && \
        chmod 755 /opt/ibm/dsdriver/installDSDriver && \
	export IBM_DB_HOME=/opt/ibm/dsdriver && \
         apt-get update && apt-get -y install ksh zip && \
         ksh /opt/ibm/dsdriver/installDSDriver && \
         env IBM_DB_HOME=/opt/ibm/dsdriver && \
         pecl install ibm_db2 && \
         echo "extension=ibm_db2.so" > /usr/local/etc/php/conf.d/ibm_db2.ini

WORKDIR /var/www/project
