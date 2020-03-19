FROM   registry.lab.example.com:5000/rhel7:7.3

MAINTAINER   Red Hat Training <training@redhat.com>

# command line options to pass to the JVM
ENV	  JAVA_OPTIONS -Xmx512m

# Need this for installing openjdk from classroom yum repo
ADD   training.repo /etc/yum.repos.d/training.repo

# Install the Java runtime, create a user for running the app, and set permissions
RUN   yum install -y --noplugins java-1.8.0-openjdk-headless && \
      yum clean all --noplugins -y && \
      mkdir -p /opt/app-root/bin

# Copy the runnable fat JAR to the container.
ADD   http://content.example.com/ocp3.6/x86_64/installers/builds-for-managers.jar /opt/app-root/bin/

COPY  run-app.sh /opt/app-root/bin/

RUN   chgrp -R 0 /opt/app-root && \
      chmod -R g=u /opt/app-root

EXPOSE 8080

USER  1001

# Run the fat JAR
CMD   /opt/app-root/bin/run-app.sh
