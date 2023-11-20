FROM php:8.0-fpm

ENV PROJECT_DIR='/scheduler-app'
ENV APP_ENV='staging'
ENV PHP_UID=1000
ENV PHP_GID=1000
ENV DOCKER_USER='www-data'

WORKDIR $PROJECT_DIR

COPY --from=composer:2.2.7 /usr/bin/composer /usr/bin/composer

RUN apt-get update \
    && apt-get install -y \
    git \
    zip \
    unzip \
    vim \
    python \
    python3-pip

RUN pip install supervisor==4.2.5

RUN apt-get update \
    && apt-get install -y libpq-dev \
    && docker-php-ext-install pdo_mysql pdo_pgsql

COPY ["./docker/staging/php/supervisor/supervisord.conf", "/etc/supervisor/supervisord.conf"]
COPY ["./docker/staging/php/supervisor/scheduler.conf", "/etc/supervisord.d/scheduler.conf"]

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"
COPY ["./docker/${APP_ENV}/php/scripts/docker-entrypoint.sh", "/"]
RUN chmod +x /docker-entrypoint.sh

COPY ["./src/", "./"]

RUN composer install

ENTRYPOINT ["/docker-entrypoint.sh"]
