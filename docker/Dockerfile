FROM ubuntu

RUN apt-get update
RUN apt-get -y install stunnel
RUN apt-get -y install net-tools

ENTRYPOINT ["/usr/bin/stunnel", "/etc/stunnel/stunnel.conf"]