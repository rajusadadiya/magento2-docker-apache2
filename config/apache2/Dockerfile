FROM php:7.2-apache
RUN docker-php-ext-install pdo pdo_mysql mysqli
RUN docker-php-ext-install bcmath
RUN apt-get -y update \
&& apt-get install -y libicu-dev \ 
&& docker-php-ext-configure intl \
&& docker-php-ext-install intl
RUN apt-get update && \
    apt-get install -y libxml2-dev
RUN apt-get update && apt-get install -y \
    libxml2-dev \
    && docker-php-ext-install soap
RUN apt-get update && \
    apt-get install -y libxslt1-dev && \
    docker-php-ext-install xsl && \
    apt-get remove -y libxslt1-dev icu-devtools libicu-dev libxml2-dev && \
    rm -rf /var/lib/apt/lists/*
# #install some base extensions FOR PHP < 7.3
RUN apt-get update \
    && apt-get install -y zlib1g-dev \
    && docker-php-ext-install zip

#install some base extensions FOR >= 7.3
#RUN apt-get install -y \
# libzip-dev \
# zip \
# && docker-php-ext-install zip

#RUN docker-php-ext-install zip

# Enable apache rewrite
COPY local.demo-com.conf /etc/apache2/sites-available/local.demo.com.conf

# Install Composer with specific composer version
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer --version=1.10.16

# Install latest Composer
#RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

#Install spatialite and create symlink for libproj.so.0
#COPY sqlite3_ext /etc/sqlite3_ext
RUN apt-get update && apt-get -y install gdal-bin
RUN ln -s /usr/lib/x86_64-linux-gnu/libproj.so.12 /usr/lib/x86_64-linux-gnu/libproj.so.0

#Install gd library for images
RUN apt-get install libpng-dev libfreetype6-dev libjpeg62-turbo-dev -qy \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd

#Copy php ini
#COPY php.ini /usr/local/etc/php/php.ini
RUN a2enmod rewrite