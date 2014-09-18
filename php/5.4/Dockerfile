FROM blinkmobile/apache-httpd:2.4.7

# update the base image
RUN apt-get update
RUN apt-get install apt-utils -y
RUN apt-get upgrade -y

# install build tools
RUN apt-get -y install git curl build-essential re2c m4

# install development headers (needed for Ruby, Node.JS and PHP)
RUN apt-get -y install libcurl4-openssl-dev libmysqlclient-dev libssl-dev libxml2-dev libxslt1-dev zlib1g-dev

# install development libraries for PHP
RUN apt-get -y build-dep php5 php5-cli
RUN apt-get -y install libedit-dev libreadline-dev libmhash-dev libt1-dev libzip2 libxpm-dev libgd2-xpm-dev libfreetype6-dev libmcrypt-dev

ENV PATH /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin

# install php-build, needed for PHP 5.4.x
RUN mkdir /opt/php-build
RUN git clone https://github.com/CHH/php-build.git /opt/php-build
RUN bash /opt/php-build/install.sh
RUN rm -fr /opt/php-build

# install PHP 5.4.32
ENV MAKEOPTS -j3
ENV EXTRA_LIBS -lstdc++ -lssl -lcrypto
ENV PHP_BUILD_CONFIGURE_OPTS --with-gettext --with-gmp-dir=/usr --with-mhash --enable-cli --with-libxml-dir=/usr --with-bz2 --enable-calendar --disable-dba --with-t1lib --with-xpm --with-freetype --enable-gd-native-ttf --with-icu-dir=/usr  --with-libedit --enable-mbregex --enable-sysvmsg --enable-wddx --with-iconv-dir=/usr  --with-pcre-regex --enable-fileinfo --enable-pdo --enable-simplexml --disable-cgi --disable-fpm --enable-intl --with-apxs2 --enable-maintainer-zts
RUN php-build --pear 5.4.32 /usr/local
RUN a2enmod php5
RUN pecl upgrade radius
RUN pecl upgrade imagick

WORKDIR /

# cleanup some unwanted packages
RUN apt-get -y remove build-essential g++ gcc
RUN apt-get -y autoremove
RUN rm -fr /tmp/php-build*

ENTRYPOINT ["/usr/local/bin/php"]
