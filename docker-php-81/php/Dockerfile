FROM php:8.1.0-fpm
RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli
RUN apt-get -y update \ 
&& apt-get install -y libicu-dev\ 
&& docker-php-ext-configure intl \ 
&& docker-php-ext-install intl \
&& apt-get install -y libssl-dev \
&& apt-get install -y libpng-dev \
&& docker-php-ext-install gd
RUN pecl install mongodb && docker-php-ext-enable mongodb

RUN apt-get update && apt-get install -y \
    libaio1 \
    unzip \
    wget

RUN curl -sS https://getcomposer.org/installer | php -- \
--install-dir=/usr/bin --filename=composer

RUN wget https://download.oracle.com/otn_software/linux/instantclient/2340000/instantclient-basic-linux.x64-23.4.0.24.05.zip \
    && wget https://download.oracle.com/otn_software/linux/instantclient/2340000/instantclient-sdk-linux.x64-23.4.0.24.05.zip
RUN unzip -o instantclient-basic-linux.x64-23.4.0.24.05.zip -d /usr/local/
RUN unzip -o instantclient-sdk-linux.x64-23.4.0.24.05.zip -d /usr/local/
RUN ln -s /usr/local/instantclient_23_4 /usr/local/instantclient
RUN rm instantclient-basic-linux.x64-23.4.0.24.05.zip \
    && rm instantclient-sdk-linux.x64-23.4.0.24.05.zip

# Menambahkan Oracle Instant Client ke LD_LIBRARY_PATH
ENV LD_LIBRARY_PATH /usr/local/instantclient

RUN docker-php-ext-configure oci8 --with-oci8=instantclient,/usr/local/instantclient \
    && docker-php-ext-install oci8

# Membersihkan cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*