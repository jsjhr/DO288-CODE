FROM registry.lab.example.com:5000/rhel7:7.3

ADD training.repo /etc/yum.repos.d
RUN yum install -y httpd \
  && yum clean all

ADD index.html /var/www/html

RUN sed -i 's/Listen 80/Listen 8080/' /etc/httpd/conf/httpd.conf \
  && chgrp -R 0 /var/log/httpd /var/run/httpd \
  && chmod -R g=u /var/log/httpd /var/run/httpd
  
EXPOSE 8080

USER 1001

ENTRYPOINT ["httpd"]
CMD  ["-D", "FOREGROUND"]

