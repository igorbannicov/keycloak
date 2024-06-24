# hadolint ignore=DL3007,DL3006
FROM registry.access.redhat.com/ubi8/openjdk-17

ARG VERSION=25.0.0
ARG PASSWORD=password

WORKDIR /opt
USER 0

RUN curl -L https://github.com/keycloak/keycloak/releases/download/$VERSION/keycloak-$VERSION.tar.gz > keycloak.tar.gz && \
    microdnf install -y gzip openssl && \
    gunzip keycloak.tar.gz && \
    tar -xvf keycloak.tar && \
    rm -rf keycloak.tar && \
    mv keycloak-$VERSION keycloak && \
    groupadd -g 1001 keycloak && \
    useradd -u 1001 -g 1001 -M keycloak && \
    mkdir /opt/certs && \
    mkdir -p /opt/keycloak/data/transaction-logs && \
    mkdir  /home/keycloak/ && \
    chown -R 1001:1001 /opt && \
    chown -R 1001:1001 /home/keycloak

USER 1001
ENV KEYCLOAK_ADMIN=admin
ENV KEYCLOAK_ADMIN_PASSWORD=password

COPY conf/keycloak.conf /opt/keycloak/conf/
COPY certs/keyStore.p12 /opt/certs/keyStore.p12
COPY certs/KeycloakKey.pem /opt/certs/KeycloakKey.pem
COPY certs/KeycloakCert.pem /opt/certs/KeycloakCert.pem
COPY conf/infinispan-keycloak-realm.json /opt/keycloak/data/import/infinispan-keycloak-realm.json

RUN /opt/keycloak/bin/kcadm.sh config truststore --trustpass $PASSWORD /opt/certs/keyStore.p12
RUN /opt/keycloak/bin/kc.sh build

ENTRYPOINT [ "/opt/keycloak/bin/kc.sh", "start", "--optimized", "--verbose"]

EXPOSE 8080
