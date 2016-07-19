FROM ku6pk/centos-base
MAINTAINER Patrick O. Kuti <code@ku6pk.ch>

ADD etc/nginx.repo /etc/yum.repos.d/nginx.repo

RUN yum -y install nginx \
  unzip

RUN yum clean all

RUN mkdir /srv/www

RUN rm -rf /etc/nginx

RUN cd /tmp && \
  curl -L -o server-configs-nginx.zip https://github.com/h5bp/server-configs-nginx/archive/master.zip && \
  unzip server-configs-nginx.zip && \
  mv server-configs-nginx-master /etc/nginx

RUN mkdir /etc/nginx/conf
RUN sed -ri 's/user www www;/user nginx nginx;\n\n# Run Nginx in the foreground for Docker.\ndaemon off;/g' /etc/nginx/nginx.conf
RUN sed -ri 's/logs\/error.log/\/var\/log\/nginx\/error.log/g' /etc/nginx/nginx.conf
RUN sed -ri 's/logs\/access.log/\/var\/log\/nginx\/access.log/g' /etc/nginx/nginx.conf
RUN sed -ri 's/logs\/static.log/\/var\/log\/nginx\/static.log/g' /etc/nginx/h5bp/location/expires.conf

RUN ln -s /etc/nginx/sites-available/no-default /etc/nginx/sites-enabled

EXPOSE 80 22

ADD scripts /scripts
RUN chmod +x /scripts/start.sh

VOLUME ["/srv/www", "/var/log", "/var/run"]

CMD ["/scripts/start.sh"]
