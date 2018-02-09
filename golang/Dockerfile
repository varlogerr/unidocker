FROM golang:1.9.4
MAINTAINER Varlog <varlog.err@gmail.com>

ENV TINI_VERSION=v0.16.1
ENV THE_USER=app
ENV THE_GROUP=app
ENV THE_HOME=/home/app

ENV GOPATH=/app

ARG THE_USER_ID
ARG THE_GROUP_ID

ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini

RUN chmod +x /tini

RUN addgroup -q --gid $THE_GROUP_ID $THE_GROUP \
    && adduser -q --disabled-login \
      --ingroup $THE_GROUP \
      --shell /bin/bash --gecos ",,," \
      --home $THE_HOME \
      --uid $THE_USER_ID $THE_USER

ENTRYPOINT ["/tini", "--"]
CMD :
