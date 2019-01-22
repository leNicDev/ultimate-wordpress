FROM alpine:3.8

# Install nginx
RUN apk add --update --no-cache nginx \
    php7-fpm \
    php7-json \
    php7-zlib \
    php7-xml \
    php7-pdo \
    php7-phar \
    php7-openssl \
    php7-pdo_mysql \
    php7-mysqli \
    php7-session \
    php7-gd \
    php7-iconv \
    php7-mcrypt \
    php7-curl \
    php7-opcache \
    php7-ctype \
    php7-apcu \
    php7-intl \
    php7-bcmath \
    php7-mbstring \
    php7-dom \
    php7-xmlreader \
    php7-redis \
    openssh-client \
    curl \
    rsync \
    musl \
    tar \
    supervisor && \
    mkdir -p /www && \
    mkdir -p /www/cache && \
    mkdir -p /www/acme && \
    mkdir -p /run/php && \
    mkdir -p /var/tmp/nginx && \
    chown -R nobody:nobody /var/tmp/nginx && \
    chown -R nobody:nobody /www

# Copy nginx configuration
COPY config/nginx.conf /etc/nginx/nginx.conf
COPY snippets /etc/nginx/snippets

# Copy PHP-FPM configuration
COPY config/fpm-pool.conf /etc/php7/php-fpm.d/zzz_custom.conf
COPY config/php.ini /etc/php7/conf.d/zzz_custom.ini

# Copy supervisord configuration
COPY config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

WORKDIR /www

# Download Wordpress
RUN curl -o /www/wordpress.tar.gz https://wordpress.org/wordpress-5.0.3.tar.gz && \
    tar -xzf wordpress.tar.gz && \
    rm wordpress.tar.gz && \
    echo 'loaderio-90ea1101655b25f8af883ed3497c3f9c' > /www/wordpress/loaderio-90ea1101655b25f8af883ed3497c3f9c.txt

# Copy Wordpress configuration
COPY config/wp-config.php /www/wordpress/wp-config.php

# Copy object cache script
COPY scripts/object-cache.php /www/wordpress/wp-content/object-cache.php

EXPOSE 80

CMD [ "/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf" ]

HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1/fpm-ping