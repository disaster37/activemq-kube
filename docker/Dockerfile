FROM redhat/ubi8-minimal:8.4

ARG http_proxy
ARG https_proxy



# Application settings
ENV \
    ACTIVEMQ_VERSION=5.17.1 \
    UID=1000 \
    GID=1000 \
    JAVA_HOME=/usr/lib/jvm/jre-1.8.0

# Install extra package
RUN \
  microdnf -y install java-1.8.0-openjdk-headless curl tar gzip bash shadow-utils &&\
  microdnf -y clean all

# Install ActiveMQ
RUN \
    curl -L https://archive.apache.org/dist/activemq/${ACTIVEMQ_VERSION}/apache-activemq-${ACTIVEMQ_VERSION}-bin.tar.gz -o /tmp/activemq.tar.gz &&\
    cd /tmp &&\
    tar -xvzf activemq.tar.gz &&\
    mv apache-activemq-* /opt/activemq &&\
    groupadd --gid ${GID} activemq && \
    useradd -c "User for activemq" -d /opt/activemq -m -g activemq -s /bin/bash -u ${UID} activemq &&\
    chown -R activemq:activemq /opt/activemq &&\
    sed -i 's|<property name="host" value=".*"/>|<property name="host" value="0.0.0.0"/>|g' /opt/activemq/conf/jetty.xml


# Clean image
RUN rm -rf  /tmp/* /var/tmp/*

WORKDIR /opt/activemq
USER activemq
EXPOSE 61616 5672 61613 1883 61614

VOLUME ["/opt/activemq/data", "/opt/activemq/log"]
CMD ["/opt/activemq/bin/activemq", "console"]