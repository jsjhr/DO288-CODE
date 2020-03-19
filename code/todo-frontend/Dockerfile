FROM registry.lab.example.com:5000/rhel7:7.3

LABEL version="1.0"
LABEL description="To Do List application front-end"
LABEL creationDate="2017-12-25"

COPY training.repo /etc/yum.repos.d
RUN yum install -y --setopt=tsflags=nodocs rh-nginx18
RUN yum clean all

COPY src /opt/rh/rh-nginx18/root/usr/share/nginx/html/

COPY nginxconf.sed /tmp/
COPY run.sh /tmp/
RUN sed -i -f /tmp/nginxconf.sed /etc/opt/rh/rh-nginx18/nginx/nginx.conf

RUN chown -R nginx:nginx /var/opt/rh/rh-nginx18 /opt/rh/rh-nginx18
RUN chmod -R a+rx,g+rx /var/opt/rh/rh-nginx18 /opt/rh/rh-nginx18
  
EXPOSE 8080

USER nginx

ENTRYPOINT ["/tmp/run.sh"]

