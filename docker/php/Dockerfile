FROM php:8.3-fpm

# Dependências de sistema
RUN apt-get update && apt-get install -y \
    git \
    curl \
    unzip \
    libzip-dev \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# Extensões PHP usadas por APIs Laravel comuns
RUN docker-php-ext-install \
    pdo \
    pdo_mysql \
    mbstring \
    exif \
    pcntl \
    bcmath \
    gd \
    zip \
    opcache

# Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www

# Usuário não-root para evitar arquivos criados como root no host
ARG UID=1000
ARG GID=1000
RUN groupadd -g ${GID} laravel \
    && useradd -u ${UID} -g laravel -m laravel

USER laravel

EXPOSE 9000

CMD ["php-fpm"]
