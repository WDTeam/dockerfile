FROM php:5.6-apache
MAINTAINER "WindsDeng" <windsdeng@gmail.com>

ADD php.ini /usr/local/etc/php/
ENV TZ=Asia/Shanghai

RUN apt-get -qq -y update && \
apt-get -qq -y --no-install-recommends install cron \
	libfreetype6-dev \
    	libjpeg62-turbo-dev \
    	libmcrypt-dev \
    	libpng12-dev \
    	libicu-dev \
    	libxml2-dev \
        wget \
	vim \
	supervisor \
	&& docker-php-ext-install -j$(nproc) iconv mcrypt intl  pdo_mysql  soap  bcmath sockets  gettext mysqli \
    	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/  \
    	&& docker-php-ext-install -j$(nproc) gd \
    	&& rm -r /var/lib/apt/lists/*

RUN a2enmod rewrite
RUN mkdir /tmp/zendguardloader && \
    cd /tmp/zendguardloader && \
    wget http://downloads.zend.com/guard/7.0.0/zend-loader-php5.6-linux-x86_64.tar.gz && \
    tar xvf zend-loader-php5.6-linux-x86_64.tar.gz && \
    cd `php -i | grep extension_dir | cut -d' ' -f 5` && \
    cp /tmp/zendguardloader/zend-loader-php5.6-linux-x86_64/ZendGuardLoader.so . && \
    cp /tmp/zendguardloader/zend-loader-php5.6-linux-x86_64/opcache.so . && \
    echo "zend_extension=ZendGuardLoader.so \nzend_loader.license_path=/var/www/html/license.zl" > /usr/local/etc/php/conf.d/10-zendguardloader.ini && \
    echo zend_extension=opcache.so > /usr/local/etc/php/conf.d/20-zendopcache.ini && \
    rm -rf /tmp/zendguardloader/

ADD crontab /etc/cron.d/hello-cron
 
RUN chmod 0644 /etc/cron.d/hello-cron && touch /var/log/cron.log
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
CMD ["/usr/bin/supervisord"]
