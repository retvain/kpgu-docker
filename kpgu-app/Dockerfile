FROM php:7.3.19-fpm

ARG HOST_UID

WORKDIR /usr/local/etc/php-fpm.d

USER root

# composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# system
RUN apt-get update && apt-get install --no-install-recommends -qy \
        openssl \
        libpq-dev \
        libxslt-dev \
        unzip \
        git \
        libzip-dev \
        locales


RUN apt-get install -y \
        build-essential \
        zlib1g-dev \
        libncurses5-dev \
        libgdbm-dev \
        libnss3-dev \
        libssl-dev \
        libsqlite3-dev \
        libreadline-dev \
        libffi-dev \
        curl \
        libbz2-dev \
        gfortran \
        libopenblas-dev \
        liblapack-dev \
        wget

WORKDIR /home

RUN wget https://www.python.org/ftp/python/3.9.4/Python-3.9.4.tgz
RUN tar -xf Python-3.9.4.tgz && cd Python-3.9.4 && ./configure --enable-optimizations && make -j 2 && make altinstall

RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py && python3.9 get-pip.py

RUN pip3 install scikit-learn==1.1.3 \
    joblib==1.1.0 \
    pandas>=1.4.4 \
    numpy>=1.21.5 \
    flask>=1.1.2 \
    waitress>=2.1.2


# Locale
RUN sed -i -e \
  's/# ru_RU.UTF-8 UTF-8/ru_RU.UTF-8 UTF-8/' /etc/locale.gen \
   && locale-gen

ENV LANG ru_RU.UTF-8
ENV LANGUAGE ru_RU:ru
ENV LC_LANG ru_RU.UTF-8
ENV LC_ALL ru_RU.UTF-8

RUN chmod 777 -R /var/www

RUN apt update && apt upgrade -y

# clean
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
WORKDIR /var/www

# xdebug
RUN pecl install xdebug-2.7.2 && docker-php-ext-enable xdebug

# db
RUN docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql && \
    docker-php-ext-install pdo_pgsql pgsql xsl

RUN docker-php-ext-install zip
ADD php.ini /usr/local/etc/php/php.ini
COPY www.conf /usr/local/etc/php-fpm.d/www.conf
RUN mkdir -p /var/www/.composer
COPY keys.dev.pub /var/www/.composer/keys.dev.pub
COPY keys.tags.pub /var/www/.composer/keys.tags.pub
COPY cacert.pem /etc/ssl/certs/cacert.pem

RUN usermod -u ${HOST_UID} www-data
USER www-data

EXPOSE 9000