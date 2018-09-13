FROM philetaylor/joomla:3.0.0

ENV APPLICATION_ENV local

COPY ./genericoldconnector /var/www/html/plugins/system/bfnetwork
COPY ./DevKeys /var/www/html/plugins/system/bfnetwork/bfnetwork/Keys
RUN chmod -Rf 777 /var/www/html

RUN apk update && apk add php5-openssl
RUN echo "extension=openssl">/etc/php5/conf.d/ssl.ini
EXPOSE 8300

