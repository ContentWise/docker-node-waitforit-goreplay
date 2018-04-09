FROM node:9-alpine

ARG GOREPLAY_VERSION=0.16.1
ARG WAITFORIT_VERSION=2.2.0

ENV SERVICE_PORT=8080
ENV MILLIS_BETWEEN_WAIT_RETRIES=2000
ENV SECONDS_TO_WAIT=86400

ENV ELASTICSEARCH_WAIT_FOR_STATUS=yellow

COPY run.sh /opt/

RUN apk update && \
        apk add ca-certificates wget && \
        update-ca-certificates
RUN wget -q -O - https://github.com/buger/goreplay/releases/download/v${GOREPLAY_VERSION}/gor_${GOREPLAY_VERSION}_x64.tar.gz | tar -xz -C /usr/local/bin
RUN wget -q -O /usr/local/bin/waitforit https://github.com/maxcnunes/waitforit/releases/download/v${WAITFORIT_VERSION}/waitforit-linux_amd64 && chmod +x /usr/local/bin/waitforit

WORKDIR /usr/src/app
ONBUILD COPY src/ /usr/src/app

USER node
CMD ["/opt/run.sh"]