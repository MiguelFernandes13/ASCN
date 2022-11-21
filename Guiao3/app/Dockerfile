# syntax=docker/dockerfile:1
FROM ubuntu:20.04
# -y => yes
RUN apt-get update &&\
    apt-get install -y software-properties-common

#install app packages
RUN add-apt-repository ppa:ondrej/php && apt-get update && apt-get install -y \
    php7.4 \
    php7.4-fpm \
    php7.4-zip \
    php7.4-mbstring \
    php7.4-tokenizer \
    php7.4-mysql \
    php7.4-gd \
    php7.4-xml \
    php7.4-bcmath \
    php7.4-intl \
    php7.4-curl

#install dependencies
RUN apt-get install -y \
    nodejs \
    composer \
    npm

#clone repo
RUN git clone https://github.com/Hackathonners/swap.git
RUN mv swap /app
WORKDIR /app

#change database config
ENV DB_HOST="swapdb"
ENV DB_DATABASE="swap"
ENV DB_USERNAME="swapuser"
ENV DB_PASSWORD="123456"
RUN cp .env.example .env
RUN sed -i 's/DB_HOST=127.0.0.1/DB_HOST=${DB_HOST}/g' /app/.env
RUN sed -i 's/DB_DATABASE=homestead/DB_DATABASE=${DB_DATABASE}/g' /app/.env
RUN sed -i 's/DB_USERNAME=homestead/DB_USERNAME=${DB_PASSWORD}/g' /app/.env
RUN sed -i 's/DB_PASSWORD=secret/DB_PASSWORD=${DB_PASSWORD}/g' /app/.env

#install packages
RUN composer install
RUN npm install

#generate the php key
RUN php artisan key:generate

#run migrations
#run seeders
#start the server

EXPOSE 8000
CMD php artisan migrate && php artisan db:seed && php artisan serve --host=0.0.0.0
