# hadolint ignore=DL3007,DL3006
FROM registry.access.redhat.com/ubi8/openjdk-17

ARG VERSION=15.0.4.Final

WORKDIR /opt
USER 0

RUN curl -L https://downloads.jboss.org/infinispan/$VERSION/infinispan-server-$VERSION.zip > infinispan.zip && \
    microdnf install -y unzip && \
    mkdir /opt/infinispan && \
    unzip infinispan.zip -d /opt/infinispan && \
    rm -rf infinispan.zip && \
    mv /opt/infinispan/infinispan-server-$VERSION/* /opt/infinispan && \
    rm -rf /opt/infinispan/infinispan-server-$VERSION && \
    groupadd -g 1001 odj_user && \
    useradd -u 1001 -g 1001 -M odj_user && \
    chown -R 1001:1001 /opt

USER 1001
COPY infinispan.xml /opt/infinispan/server/conf/infinispan.xml

ENTRYPOINT [ "/opt/infinispan/bin/server.sh"]

EXPOSE 8080
