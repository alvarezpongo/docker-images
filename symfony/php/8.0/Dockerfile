FROM php:8.0-rc-fpm-alpine

LABEL org.opencontainers.image.authors="Alexandre Jardin <info@ajardin.fr>"
LABEL org.opencontainers.image.title="PHP 8.0 image for a Symfony application."
LABEL org.opencontainers.image.description="https://symfony.com/"
LABEL org.opencontainers.image.source="https://github.com/ajardin/docker-images/tree/master/symfony/php"

# Install Symfony requirements
RUN \
    apk update && \
    apk add --no-cache \
        bash \
        freetype-dev \
        git \
        icu-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        libwebp-dev \
        libzip-dev \
        openssh-client \
        patch \
        perl \
        postgresql-libs \
        shadow \
        ssmtp \
        yarn && \
    apk add --no-cache --virtual .build-deps \
		$PHPIZE_DEPS \
		postgresql-dev && \
    docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp && \
    docker-php-ext-install -j$(nproc) \
        intl \
        gd \
        opcache \
        pdo_mysql \
        pdo_pgsql \
        zip && \
    perl -pi -e "s/mailhub=mail/mailhub=maildev/" /etc/ssmtp/ssmtp.conf && \
    apk del .build-deps

# Install the Blackfire PHP probe
#RUN \
#    blackfire_probe_version=1.34.3 && \
#    php_version=$(php -r "echo PHP_MAJOR_VERSION.PHP_MINOR_VERSION;") && \
#    curl https://packages.blackfire.io/binaries/blackfire-php/${blackfire_probe_version}/blackfire-php-alpine_amd64-php-${php_version}.so \
#        -sS --output $(php -r "echo ini_get('extension_dir');")/blackfire.so && \
#    docker-php-ext-enable blackfire

# Assign a new UID/GID to avoid using a generated value
RUN \
    usermod -u 1000 www-data && \
    groupmod -g 1000 www-data

# Install Composer globally
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_MEMORY_LIMIT -1

# Install custom entrypoint
COPY --from=ajardin/blackfire-entrypoint:latest /usr/local/bin/docker-custom-entrypoint /usr/local/bin/docker-custom-entrypoint
RUN chmod 777 /usr/local/bin/docker-custom-entrypoint
CMD ["php-fpm"]
ENTRYPOINT ["docker-custom-entrypoint"]