FROM composer:1.8.4 AS composer

FROM php:7.2-cli-alpine3.6
LABEL maintainer Joseph Ligier

RUN apk --update --no-cache add \
	curl \
	freetype-dev \
	g++ \
	git \
	icu \
	icu-dev \
	libjpeg-turbo-dev \
	libpng-dev \
	libxml2-dev \
	libxslt-dev \
	shadow \
	tree \
	vim \
	rsync \
	zlib-dev

RUN docker-php-ext-configure \
	gd --with-jpeg-dir=/usr/local/ --with-freetype-dir=/usr/include/
RUN docker-php-ext-install \
	gd \
	bcmath \
	intl \
	pdo_mysql \
	soap \
	xsl \
	zip

RUN addgroup -g 1000 magento && \
	adduser -D -S -G magento -u 1000 -s /bin/sh magento

COPY --from=composer /usr/bin/composer /usr/bin/composer

COPY ./conf/php.ini /usr/local/etc/php/php.ini
COPY ./conf/aliases.sh /etc/profile.d/aliases.sh


ADD bin/startup.sh /usr/local/bin
RUN chmod a+x /usr/local/bin/startup.sh

WORKDIR /var/tmp

USER magento
ADD --chown=magento:magento cinemanext-eshop/ /var/tmp/
RUN mkdir /var/tmp/vendor/magento/
RUN composer install

WORKDIR /var/www/html
ENTRYPOINT [ "/usr/local/bin/startup.sh" ]
