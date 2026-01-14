FROM php:8.2-fpm

LABEL author="Ashu" maintainer="Matheusn.biolowons@gmail.com"
ENV PHP_VERSION="8.2"

ADD --chmod=0755 https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

# System dependencies + MySQL client
RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        wget \
        jq \
        tar \
        curl \
        nginx \
        unzip \
        git \
        bash \
        fakeroot \
        libsodium-dev \
        libzip-dev \
        zip \
        libpng-dev \
        libicu-dev \
        default-mysql-client \
        lolcat \
        figlet && \
    rm -rf /var/lib/apt/lists/*

# PHP extensions
RUN docker-php-ext-install \
    zip \
    intl \
    sodium \
    pdo \
    pdo_mysql \
    mysqli \
    gd \
    bcmath

# PHP-FPM symlink (safe even if unused)
RUN ln -sf /usr/local/sbin/php-fpm /usr/sbin/php-fpm

# Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Timezone
RUN echo "UTC" > /etc/timezone

# Cron script
COPY ./crontab_test.sh /crontab_test.sh
RUN chmod +x /crontab_test.sh

ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8

# Container user
USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

# Entrypoint
COPY ./entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Create container user
RUN useradd -m -d /home/container -s /bin/bash container

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

CMD ["/bin/bash", "/entrypoint.sh"]
