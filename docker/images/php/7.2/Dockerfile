# Для начала указываем исходный образ, он будет использован как основа
FROM php:7.2-fpm

# RUN выполняет идущую за ней команду в контексте нашего образа.
# В данном случае мы установим некоторые зависимости и модули PHP.
# Для установки модулей используем команду docker-php-ext-install.
# На каждый RUN создается новый слой в образе, поэтому рекомендуется объединять команды.
RUN set -xe && \
	apt-get update && apt-get install -y \
	curl \
	wget \
	git \
	nano \
	locales \
	htop \
	mc \
	libpng-dev \
	libicu-dev \
	libpq-dev \
#Установка расширений
	&& docker-php-ext-install -j$(nproc) pgsql pdo_pgsql intl pcntl gd opcache \
#Очистка мусора
	&& apt-get autoremove -y \
    	&& apt-get clean && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*

# Куда же без composer'а.
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('SHA384', 'composer-setup.php') === trim(file_get_contents('https://composer.github.io/installer.sig'))) { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

# Добавим свой php.ini, можем в нем определять свои значения конфига
ADD php.ini /usr/local/etc/php/conf.d/40-custom.ini

RUN sed -i "s/^# en_US.UTF-8/en_US.UTF-8/" /etc/locale.gen \
    && sed -i "s/^# ru_RU.UTF-8/ru_RU.UTF-8/" /etc/locale.gen \
    && locale-gen

#RUN usermod -u 1000 www-data \
#    && chown -R www-data:www-data /var/www

ENV LANG ru_RU.UTF-8
ENV LANGUAGE ru_RU:en
ENV LC_ALL ru_RU.UTF-8

RUN usermod -u 1000 www-data \
    && chown -R www-data:www-data /var/www

# Указываем рабочую директорию для PHP
WORKDIR /var/www/html

USER www-data:www-data
