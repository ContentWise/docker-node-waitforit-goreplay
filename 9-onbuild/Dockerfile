FROM golang:alpine AS waitforit
RUN apk add --no-cache git
RUN go get github.com/ContentWise/waitforit

FROM node:9-alpine

ARG GOREPLAY_VERSION=0.16.1

ONBUILD ARG GIT_BRANCH
ONBUILD ARG GIT_BUILD_TIME
ONBUILD ARG GIT_BUILD_VERSION
ONBUILD ARG GIT_COMMIT_ID
ONBUILD ARG GIT_COMMIT_ID_ABBREV
ONBUILD ARG GIT_COMMIT_TIME

ONBUILD ENV GIT_BRANCH=${GIT_BRANCH}
ONBUILD ENV GIT_BUILD_TIME=${GIT_BUILD_TIME}
ONBUILD ENV GIT_BUILD_VERSION=${GIT_BUILD_VERSION}
ONBUILD ENV GIT_COMMIT_ID=${GIT_COMMIT_ID}
ONBUILD ENV GIT_COMMIT_ID_ABBREV=${GIT_COMMIT_ID_ABBREV}
ONBUILD ENV GIT_COMMIT_TIME=${GIT_COMMIT_TIME}

ENV SERVICE_PORT=8080
ENV MILLIS_BETWEEN_WAIT_RETRIES=2000
ENV SECONDS_TO_WAIT=86400

ENV ELASTICSEARCH_WAIT_FOR_STATUS=yellow

COPY run.sh /opt/

RUN apk update && \
        apk add ca-certificates wget && \
        update-ca-certificates
RUN wget -q -O - https://github.com/buger/goreplay/releases/download/v${GOREPLAY_VERSION}/gor_${GOREPLAY_VERSION}_x64.tar.gz | tar -xz -C /usr/local/bin
COPY --from=waitforit /go/bin/waitforit /usr/local/bin/waitforit

WORKDIR /usr/src/app
ONBUILD COPY src/ /usr/src/app

USER node
CMD ["/opt/run.sh"]
