FROM centos:centos7
MAINTAINER Patrick O. Kuti <code@ku6pk.ch>

RUN yum -y install http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm \
  yum -y upgrade

RUN yum -y install \
  bash-completion \
  curl \
  hostname \
  initscripts \
  openssh-clients \
  openssh-server \
  vim-enhanced \
  tmux \
  sudo \
  syslog-ng \
  syslog-ng-libdbi \
  yum-plugin-fastestmirror

RUN yum clean all

EXPOSE 22

ADD etc/sudoers /etc/sudoers
RUN chmod 440 /etc/sudoers

ADD scripts /scripts
RUN chmod +x /scripts/start.sh
RUN touch /first_run
RUN echo "UseDNS no" >> /etc/ssh/sshd_config
RUN sed -i 's/UsePrivilegeSeparation sandbox/UsePrivilegeSeparation no/' /etc/ssh/sshd_config
RUN sed -i 's/ClientAliveCountMax 3/ClientAliveCountMax 0/' /etc/ssh/sshd_config
RUN sed -i 's/ClientAliveInterval 0/ClientAliveInterval 600/' /etc/ssh/sshd_config

RUN echo 'root:$h0uldChang3Th1$' | chpasswd

VOLUME ["/var/log", "/run"]

CMD ["/scripts/start.sh"]
