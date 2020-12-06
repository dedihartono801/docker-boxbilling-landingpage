#Download base image ubuntu 18.04
FROM ubuntu:18.04
 
# Update Software repository
RUN apt-get update && apt-get upgrade

RUN apt-get install -y software-properties-common
RUN add-apt-repository ppa:ondrej/php

# Install nginx, php-fpm, extention and supervisord from ubuntu repository
RUN apt-get update && apt-get install -y nginx php7.4-fpm supervisor php-mysql php-common php-cli php7.4-curl zlib1g-dev gettext php-mbstring && \
    rm -rf /var/lib/apt/lists/*
 
#Define the ENV variable
ENV nginx_vhost /etc/nginx/sites-available/default
ENV php_conf /etc/php/7.4/fpm/php.ini
ENV nginx_conf /etc/nginx/nginx.conf
ENV supervisor_conf /etc/supervisor/supervisord.conf
 
# Enable php-fpm on nginx virtualhost configuration
COPY default ${nginx_vhost}
RUN sed -i -e 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' ${php_conf} && \
    echo "\ndaemon off;" >> ${nginx_conf}
 
#Copy supervisor configuration
COPY supervisord.conf ${supervisor_conf}
 
RUN mkdir -p /run/php && \
    chown -R www-data:www-data /var/www/html && \
    chown -R www-data:www-data /run/php
 
# Volume configuration
VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx", "/var/www/html"]
 
# Configure Services and Port
COPY start.sh /start.sh
CMD ["./start.sh"]
 
EXPOSE 80