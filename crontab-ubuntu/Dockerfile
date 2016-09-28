FROM ubuntu:latest
MAINTAINER "WindsDeng" <windsdeng@gmail.com>

RUN apt-get -qq -y update && \
apt-get -qq -y --no-install-recommends install cron 
 
ADD crontab /etc/cron.d/hello-cron
 
RUN echo http://www.dlf5.com
 
RUN chmod 0644 /etc/cron.d/hello-cron
 
RUN touch /var/log/cron.log
 
CMD cron && tail -f /var/log/cron.log

