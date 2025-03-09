FROM jenkins/jenkins:lts

USER root

# Installation de PHP et Composer
RUN apt-get update && apt-get install -y \
    php-cli \
    php-mbstring \
    php-xml \
    php-tokenizer \
    php-curl \
    git \
    curl \
    unzip \
    && rm -rf /var/lib/apt/lists/*

# Installation de Composer 
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Installation de PHPCS, PHPMD et PHPStan via Composer global
RUN composer global require \
    "squizlabs/php_codesniffer" \
    "phpmd/phpmd" \
    "phpstan/phpstan" \
    && rm -rf /var/lib/apt/lists/*

# Ajout des binaires de Composer au PATH
ENV PATH="/root/.composer/vendor/bin:$PATH"

USER jenkins

