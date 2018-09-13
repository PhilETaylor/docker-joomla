FROM philetaylor/joomla:3.4.0

ENV APPLICATION_ENV local

COPY ./genericoldconnector /var/www/html/plugins/system/bfnetwork
COPY ./DevKeys /var/www/html/plugins/system/bfnetwork/bfnetwork/Keys
RUN chmod -Rf 777 /var/www/html

EXPOSE 8340

