FROM ubuntu:24.04

# Metadata
LABEL name="c-db-01"
LABEL company="ACME Infinity Servers"

# Variables
ENV DEBIAN_FRONTEND=noninteractive
ARG DB_ADMINER_FILE="/var/www/html/database.sql"
ARG DB_CUSTOMERS_FILE="/var/tmp/customers.sql"

# Install Required Packages
RUN apt-get update -y --fix-missing
RUN apt-get install -y \
    cron \
    mariadb-server \
    apache2 \
    php \
    php-mysql \
    lshw \
    at 

#Install Wazuh Agent
RUN curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | apt-key add -
RUN echo "deb https://packages.wazuh.com/4.x/apt/ stable main" | tee /etc/apt/sources.list.d/wazuh.list
RUN apt-get update -y --fix-missing
RUN apt-get install wazuh-agent -y

# Post Actions
COPY files/adminer-4.8.1.php /var/www/html/adminer.php
COPY files/database.sql $DB_ADMINER_FILE
COPY files/customers.sql $DB_CUSTOMERS_FILE
COPY ossec.conf /var/ossec/etc/ossec.conf

RUN mkdir /var/www/html/uploads 
RUN chmod 777 /var/www/html/uploads

RUN service mariadb restart \
    && \
    cat "$DB_ADMINER_FILE" | mysql -u root \
    && \
    cat "$DB_CUSTOMERS_FILE" | mysql -u root && rm "$DB_CUSTOMERS_FILE"
    
# Entry
ENTRYPOINT service mariadb restart \
    && \
    /usr/sbin/apache2ctl -D FOREGROUND