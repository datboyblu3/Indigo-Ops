FROM lab_base

# Metadata
LABEL name="c-backup-01"
LABEL company="ACME Infinity Servers"

# Install Required Packages
RUN apt-get update -y --fix-missing
RUN apt-get install -y python3 


#Install Wazuh Agent
RUN curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | apt-key add -
RUN echo "deb https://packages.wazuh.com/4.x/apt/ stable main" | tee /etc/apt/sources.list.d/wazuh.list
RUN apt-get update -y --fix-missing
RUN apt-get install wazuh-agent -y

    # Post Actions
COPY files/execute.sh /mnt/scripts/
COPY ossec.conf /var/ossec/etc/ossec.conf

RUN chmod u+x /mnt/scripts/execute.sh
RUN echo '*/1 * * * * bash /mnt/scripts/execute.sh' >> /tmp/root-crontab
RUN crontab /tmp/root-crontab && rm /tmp/root-crontab
RUN mkdir -p /var/www/site

ENTRYPOINT service cron restart && python3 -m http.server --directory /var/www/site 8080

