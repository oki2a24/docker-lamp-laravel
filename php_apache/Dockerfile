From php:apache
RUN apt-get update && apt-get install -y \
    unzip \
    zlib1g-dev \
    && docker-php-ext-install \
    zip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
RUN curl -s http://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer
