# The line below states we will base our new image on the Latest Official Ubuntu 
FROM ubuntu:18.04

ENV MEMORY_LIMIT "128M"

# Identify the maintainer of an image
MAINTAINER My Name "info@juicyapphk.com"

# Update the image to the latest packages
RUN apt update && apt upgrade -y

# Install apt-utils
RUN apt -y install apt-utils

# Set timezone & hostname
ENV TZ Asia/Hong_Kong
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# RUN hostnamectl set-hostname 'ubuntu18_apache2_php7'

# Install vim apache2 and modules
RUN apt install -y apache2 libapache2-mod-php7.2 && a2enmod php7.2 && a2enmod rewrite && a2enmod headers && a2enmod proxy && a2enmod ssl

# Install PHP 7.2+ and modules
RUN apt install -y software-properties-common
RUN apt install -y php7.2 php7.2-fpm php7.2-common php7.2-mysql php7.2-curl php7.2-cli php7.2-gd php7.2-mbstring php7.2-xml php7.2-soap php7.2-intl

# Add user and associated to www-data group
RUN adduser juicyapp
RUN usermod -aG sudo juicyapp
RUN chown -R juicyapp:www-data /var/www/
RUN chmod 755 -R /var/www/

# TODO:: Install MariaDB

# Apache config file
COPY ./000-default.conf /etc/apache2/sites-available/000-default.conf

# Set php memory limit
RUN sed -i -r "s/memory_limit = 128M/memory_limit = $MEMORY_LIMIT/g" /etc/php/7.2/apache2/php.ini

# Mount Volumes
VOLUME "/var/www/html/" 

# Expose ports
EXPOSE 80 443

# Start apache2 and tail to keeps container running
CMD /bin/bash service apache2 start && tail -f /dev/null
