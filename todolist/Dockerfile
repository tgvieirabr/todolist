FROM php:7.4-apache

# Adiciona a diretiva ServerName ao arquivo de configuração do Apache
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

# Instalando extensões necessárias e o pacote de desenvolvimento do PostgreSQL
RUN apt-get update && apt-get install -y \
    libpq-dev \
    && docker-php-ext-install pdo pdo_pgsql

# Habilitando mod_rewrite
RUN a2enmod rewrite

# Download e configuração do CodeIgniter
WORKDIR /var/www/html
RUN apt-get install -y wget unzip \
    && wget https://api.github.com/repos/bcit-ci/CodeIgniter/zipball/refs/tags/3.1.13 -O codeigniter.zip \
    && unzip codeigniter.zip \
    && rm codeigniter.zip \
    && mv bcit-ci-CodeIgniter-*/* . \
    && rm -r bcit-ci-CodeIgniter-* \
    && mkdir writable \
    && chown -R www-data:www-data /var/www/html \
    && chmod -R 775 /var/www/html/writable

# Permissões necessárias
RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 775 /var/www/html/writable

# Configuração do DocumentRoot
ENV APACHE_DOCUMENT_ROOT /var/www/html/
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf \
    && sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}/!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf
