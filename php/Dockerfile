FROM php:7.0-fpm

RUN apt-get update && apt-get install -y libmcrypt-dev libxslt-dev libicu-dev zlib1g-dev libpng-dev libjpeg-dev libwebp-dev libjpeg62-turbo-dev libfreetype6-dev
RUN docker-php-ext-install pdo_mysql mysqli mcrypt xsl intl zip soap
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && docker-php-ext-install -j$(nproc) gd

COPY custom.ini /usr/local/etc/php/conf.d/
COPY php-fpm.conf /usr/local/etc/php/

RUN mkdir -p /var/log/php-fpm

RUN apt-get -y install openssh-server supervisor

RUN mkdir /var/run/sshd
RUN echo 'root:smtestpassword' | chpasswd
RUN sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
EXPOSE 22
CMD ["/usr/bin/supervisord"]
