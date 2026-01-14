FROM node:20-alpine AS node
FROM alpine:3.20

LABEL author="Ashu" maintainer="Matheusn.biolowons@gmail.com"

ENV PHP_VERSION=82
ENV USER=container
ENV HOME=/home/container

# Copy Node from builder
COPY --from=node /usr/lib /usr/lib
COPY --from=node /usr/local/share /usr/local/share
COPY --from=node /usr/local/lib /usr/local/lib
COPY --from=node /usr/local/include /usr/local/include
COPY --from=node /usr/local/bin /usr/local/bin

# Base packages
RUN apk add --no-cache \
    ca-certificates \
    wget \
    jq \
    tar \
    curl \
    nginx \
    unzip \
    git \
    bash \
    yarn \
    fakeroot

# Extra tools
RUN apk add --no-cache \
    --repository=http://dl-cdn.alpinelinux.org/alpine/edge/community \
    --repository=http://dl-cdn.alpinelinux.org/alpine/edge/testing \
    figlet lolcat

# PHP
RUN echo "**** Installing php${PHP_VERSION} ****" && \
    apk add --no-cache \
    php${PHP_VERSION} \
    php${PHP_VERSION}-bcmath \
    php${PHP_VERSION}-cli \
    php${PHP_VERSION}-common \
    php${PHP_VERSION}-dom \
    php${PHP_VERSION}-curl \
    php${PHP_VERSION}-intl \
    php${PHP_VERSION}-fileinfo \
    php${PHP_VERSION}-fpm \
    php${PHP_VERSION}-gd \
    php${PHP_VERSION}-mbstring \
    php${PHP_VERSION}-mysqli \
    php${PHP_VERSION}-pdo \
    php${PHP_VERSION}-pdo_mysql \
    php${PHP_VERSION}-phar \
    php${PHP_VERSION}-posix \
    php${PHP_VERSION}-session \
    php${PHP_VERSION}-simplexml \
    php${PHP_VERSION}-sodium \
    php${PHP_VERSION}-tokenizer \
    php${PHP_VERSION}-xml \
    php${PHP_VERSION}-zip \
    php${PHP_VERSION}-iconv && \
    ln -s /usr/bin/php${PHP_VERSION} /usr/bin/php && \
    ln -s /usr/sbin/php-fpm${PHP_VERSION} /usr/sbin/php-fpm

# Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Timezone
RUN echo "UTC" > /etc/timezone

# Create user
RUN adduser -D -h /home/container container

# Scripts
COPY ./crontab_test.sh /crontab_test.sh
RUN chmod +x /crontab_test.sh

COPY ./entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

WORKDIR /home/container
USER container

CMD ["bash", "/entrypoint.sh"]
