FROM ubuntu:20.04

# Set the working directory
WORKDIR /var/www/html/

# Use the default UTF-8 language.
ENV LC_ALL=C.UTF-8 

ARG DEBIAN_FRONTEND=noninteractive
ENV CONTAINER_TIMEZONE="Asia/Kolkata"
RUN ln -snf /usr/share/zoneinfo/$CONTAINER_TIMEZONE /etc/localtime && echo $CONTAINER_TIMEZONE > /etc/timezone

# Install dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    nano \
    git \
    curl \
    python3 \
    zip \
    unzip \
    sudo \
    gpg \
    wget \
    gnupg

RUN apt-get update
RUN apt-get upgrade -y
RUN apt install -y apache2
RUN apt install -y software-properties-common
RUN add-apt-repository -y ppa:ondrej/php
RUN apt-get update
RUN apt-get install -y php7.4 libapache2-mod-php7.4 php7.4-cli php7.4-common php7.4-mbstring php7.4-gd php7.4-intl php7.4-xml php7.4-mysql php7.4-mcrypt php7.4-zip php7.4-mongodb php7.4-curl
RUN apt-get install -y php8.2 libapache2-mod-php8.2 php8.2-cli php8.2-common php8.2-mysql php8.2-pgsql php8.2-sqlite3 php8.2-xml php8.2-mbstring php8.2-curl php8.2-gd php8.2-imagick php8.2-zip php8.2-bcmath php8.2-mongodb

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

#Install node js v16.x
RUN curl -fsSL https://deb.nodesource.com/setup_16.x | bash -
RUN apt-get install -y nodejs
RUN npm install pm2 -g

# Clear cache
RUN apt-get clean

RUN echo "#!/bin/sh" > startserver
RUN echo "service apache2 start" >> startserver
RUN chmod +x startserver

RUN mv startserver /usr/bin/

#Enable php7.4
RUN a2dismod php8.2
RUN a2enmod php7.4
RUN update-alternatives --set php /usr/bin/php7.4

# Modify the Apache configuration to allow .htaccess files
RUN a2enmod rewrite
RUN sed -i 's/AllowOverride None/AllowOverride All/' /etc/apache2/apache2.conf
RUN service apache2 restart

# Start Apache in the foreground
CMD startserver; sleep infinity; apache2-foreground;