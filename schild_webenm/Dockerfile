FROM php:8.0-apache
ARG REPO=schild_webenm
WORKDIR /var/www/html

# copy necessary files into container
COPY ${REPO}/*.php .
COPY ${REPO}/.htaccess .
COPY ${REPO}/composer.json .
COPY ${REPO}/composer.lock .
COPY ${REPO}/config config
COPY ${REPO}/css css
COPY ${REPO}/grade-files grade-files
COPY ${REPO}/img img
COPY ${REPO}/includes includes
COPY ${REPO}/js js
COPY ${REPO}/lib lib
RUN mkdir -p grade-files/tmp
RUN mkdir -p grade-files/working
RUN chmod a+rw grade-files/tmp
RUN chmod a+rw grade-files/working

# composer
RUN apt-get update && apt-get install -y unzip
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer
RUN composer install --no-dev

# install php extensions
RUN apt-get install -y libzip-dev
RUN docker-php-ext-install zip

# install apache extensions
RUN a2enmod rewrite && \
    a2enmod headers && \
    service apache2 restart