FROM alpine:3.8

ARG JOOMLA_VERSION="3.0.0"
ARG JOOMLA_VERSION_NODOTS="300"
ARG JOOMLA_INSTALLURL="https://mirror.myjoomla.io/Joomla_3.0.0-Stable-Full_Package.zip"

ENV MYSQL_ROOT_PWD="dbpassword"
ENV MYSQL_USER="dbuser"
ENV MYSQL_USER_PWD="dbpassword"
ENV MYSQL_USER_DB="joomla$JOOMLA_VERSION_NODOTS"

RUN apk update

# PHP

RUN apk add --no-cache  \
    php5              \
    php5-fpm          \
    php5-curl         \
    php5-ctype        \
    php5-zip          \
#   php5-mbstring     \
    php5-pcntl        \
    php5-posix        \
    php5-iconv        \
    php5-intl         \
#   php5-session      \
    php5-pdo_mysql    \
    php5-mysqli       \
#   php5-tokenizer    \
    php5-dom          \
    php5-xml          \
#   php5-simplexml    \
    php5-json         \
#   php5-sodium       \
    php5-opcache      \
    php5-shmop        \
    php5-zlib         \
#   php5-xmlwriter    \
    php5-xmlreader    \
    php5-xml          \
    php5-common       \
    supervisor          \
    git                 \
    openssh             \
    ca-certificates     \
    curl                \
    wget                \
    zlib-dev            \
    unzip               \
    tzdata              \
    nginx               \
    procps

RUN update-ca-certificates

# PHP Configuration
RUN echo 'memory_limit=1024M' > /etc/php5/conf.d/memory_limit.ini
RUN echo 'output_buffering=Off' > /etc/php5/conf.d/output_buffering.ini
RUN echo '[global]' > /etc/php5/fpm.d/zz-docker.conf
RUN echo 'daemonize = yes' >> /etc/php5/fpm.d/zz-docker.conf
RUN echo '[www]' >> /etc/php5/fpm.d/zz-docker.conf
RUN echo 'listen=9000' >> /etc/php5/fpm.d/zz-docker.conf
RUN echo 'realpath_cache_size=2048M' > /etc/php5/conf.d/pathcache.ini
RUN echo 'realpath_cache_ttl=7200' >> /etc/php5/conf.d/pathcache.ini
RUN echo '[opcache]' > /etc/php5/conf.d/opcache.ini
RUN echo 'opcache.memory_consumption = 512M' >> /etc/php5/conf.d/opcache.ini
RUN echo 'opcache.max_accelerated_files = 1000000' >> /etc/php5/conf.d/opcache.ini

# Install Mysql
RUN apk add --no-cache mysql mysql-client
RUN addgroup mysql mysql
RUN echo "lower_case_table_names=1 >> /etc/mysql/conf.d/docker.cnf"
RUN mkdir /scripts
COPY ./mysql.sh /scripts/mysql.sh
RUN chmod +x /scripts/mysql.sh
RUN /scripts/mysql.sh

RUN mkdir -p /run/nginx/
RUN mkdir -p /var/log/nginx/
COPY site.conf /etc/nginx/conf.d/site.conf
COPY nginx.conf /etc/nginx/nginx.conf
RUN rm /etc/nginx/conf.d/default.conf

#Supervisor
COPY supervisord.conf /etc/supervisord.conf
RUN mkdir -p /var/log/supervisord/

# needed for mysql
RUN chmod 777 /var/tmp

# Docker
RUN ln -sf /dev/stdout /var/log/nginx/8$JOOMLA_VERSION_NODOTS.log
RUN ln -sf /dev/stdout /var/log/nginx/8$JOOMLA_VERSION_NODOTS.log

#Joomla
ADD $JOOMLA_INSTALLURL /tmp/source
RUN unzip /tmp/source -d /var/www/html/
RUN chown nginx:nginx /var/www/html
RUN chmod 777 /var/www/html

EXPOSE 8$JOOMLA_VERSION_NODOTS

CMD ["supervisord", "-c", "/etc/supervisord.conf"]
