FROM php:5.6-apache

# Switch to archived Debian repositories and remove stretch-updates
RUN sed -i 's|http://deb.debian.org/debian|http://archive.debian.org/debian|g' /etc/apt/sources.list && \
    sed -i 's|http://security.debian.org/debian-security|http://archive.debian.org/debian-security|g' /etc/apt/sources.list && \
    sed -i '/stretch-updates/d' /etc/apt/sources.list && \
    apt-get update
    
# Update package lists
# Install required PHP extensions
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libcurl4-openssl-dev \ 
    libxml2-dev \  
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd mysql mysqli pdo pdo_mysql zip curl mbstring xml


# Enable mod_rewrite
RUN a2enmod rewrite

# Set the working directory
WORKDIR /var/www/html

# Copy custom virtual host configuration
COPY vhosts /etc/apache2/sites-available/

# Enable virtual hosts
RUN a2ensite hiddenbeachesofnosara.conf hiddenbeachesofnosara.site-domain.conf \
    && service apache2 restart

# Install necessary extensions
# RUN docker-php-ext-install mysql mysqli pdo pdo_mysql

# Copy custom PHP configuration
COPY custom.ini /usr/local/etc/php/conf.d/custom.ini

 # Expose Apache ports
EXPOSE 80

# Start Apache in foreground
CMD ["apache2-foreground"]