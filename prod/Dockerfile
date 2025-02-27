FROM debian:12

# Configuración do entorno
ENV DEBIAN_FRONTEND=noninteractive

# Instalar Apache, PHP e as extensións necesarias
RUN apt-get update && apt-get install -y \
    apache2 \
    php \
    php-mysqli \
    libapache2-mod-php \
    curl \
    vim \
    git \
    unzip \
    openssl && apt-get clean && rm -rf /var/lib/apt/lists/*

# Habilitar Apache
RUN echo "<?php phpinfo(); ?>" > /var/www/html/index.php

# Habilitar modulos SSL en Apache
RUN a2enmod ssl

# Copiar los certificados SSL al contenedor (suponiendo que están en ./certs)
COPY ./certs/sitio.pem /etc/ssl/certs/sitio.pem
COPY ./certs/sitio.key /etc/ssl/private/sitio.key

# Crear un archivo de configuración para habilitar SSL en Apache
RUN echo '<VirtualHost *:443>' > /etc/apache2/sites-available/default-ssl.conf && \
    echo '    DocumentRoot /var/www/html' >> /etc/apache2/sites-available/default-ssl.conf && \
    echo '    ServerName localhost' >> /etc/apache2/sites-available/default-ssl.conf && \
    echo '    SSLEngine on' >> /etc/apache2/sites-available/default-ssl.conf && \
    echo '    SSLCertificateFile /etc/ssl/certs/sitio.pem' >> /etc/apache2/sites-available/default-ssl.conf && \
    echo '    SSLCertificateKeyFile /etc/ssl/private/sitio.key' >> /etc/apache2/sites-available/default-ssl.conf && \
    echo '    SSLProtocol all -SSLv2 -SSLv3' >> /etc/apache2/sites-available/default-ssl.conf && \
    echo '    SSLCipherSuite HIGH:!aNULL:!MD5' >> /etc/apache2/sites-available/default-ssl.conf && \
    echo '</VirtualHost>' >> /etc/apache2/sites-available/default-ssl.conf

# Habilitar el sitio SSL y reiniciar Apache
RUN a2ensite default-ssl.conf && \
    service apache2 restart

# Expor os portos 80 e 443
EXPOSE 80
EXPOSE 443

# Comando por defecto para ejecutar Apache en primer plano
CMD ["apachectl", "-D", "FOREGROUND"]
