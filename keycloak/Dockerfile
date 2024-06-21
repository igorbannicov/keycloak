# hadolint ignore=DL3007,DL3006
FROM registry.access.redhat.com/ubi8/openjdk-17

ARG VERSION=25.0.0

WORKDIR /opt
USER 0

RUN curl -L https://github.com/keycloak/keycloak/releases/download/$VERSION/keycloak-$VERSION.tar.gz > keycloak.tar.gz && \
    microdnf install -y gzip openssl && \
    gunzip keycloak.tar.gz && \
    tar -xvf keycloak.tar && \
    rm -rf keycloak.tar && \
    mv keycloak-$VERSION keycloak && \
    groupadd -g 1001 odj_user && \
    openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 \
    -nodes -keyout /opt/keycloak/server.key -out /opt/keycloak/server.crt -subj "/CN=localhost" \
    -addext "subjectAltName=DNS:localhost,DNS:*.localhost" && \
    useradd -u 1001 -g 1001 -M odj_user && \
    chown -R 1001:1001 /opt



USER 1001
COPY keycloak.conf /opt/keycloak/conf/

RUN /opt/keycloak/bin/kc.sh build

ENTRYPOINT [ "/opt/keycloak/bin/kc.sh", "start", "--optimized", "--verbose"]

EXPOSE 8080