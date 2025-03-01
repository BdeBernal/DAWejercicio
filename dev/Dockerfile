FROM debian:12

# Actualizar paquetes e instalar utilidades básicas
RUN apt update && apt -y install wget curl sudo lsb-release apt-transport-https ca-certificates \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

# Instalar Apache e PHP 8.1 con todos os módulos necesarios
RUN apt update && apt install -y apache2 php8.1 php8.1-mysql \
    php8.1-xml \
    php8.1-gd \
    php8.1-curl \
    php8.1-mbstring \
    php8.1-zip \
    php8.1-intl \
    php8.1-bcmath \
    php8.1-opcache \
    php8.1-soap \
    php8.1-pdo \
    php8.1-pdo-mysql \
    php8.1-json \
    php8.1-simplexml \
    git \
    unzip \
    libnss3-tools \
    xdg-utils \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

# Habilitar módulos de Apache necesarios
RUN a2enmod rewrite headers ssl

# Configurar Apache para PrestaShop
COPY prestashop.conf /etc/apache2/sites-available/prestashop.conf
RUN a2dissite 000-default.conf && a2ensite prestashop.conf

# Definir o directorio de traballo
WORKDIR /var/www/html

# Expor os portos 80 e 443
EXPOSE 80 443

# Iniciar Apache
CMD ["apachectl", "-D", "FOREGROUND"]
