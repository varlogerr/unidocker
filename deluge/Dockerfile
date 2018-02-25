FROM debian:9.3
MAINTAINER Varlog <varlog.err@gmail.com>

ENV TINI_VERSION=v0.16.1
ENV THE_USER=app
ENV THE_GROUP=app
ENV THE_HOME=/app

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

RUN apt-get update \
    && apt-get install -y software-properties-common

RUN add-apt-repository 'deb http://ppa.launchpad.net/deluge-team/ppa/ubuntu xenial main' \
    && apt-get update \
    && apt-get install --allow-unauthenticated -qy deluged deluge-web \
    && apt-get -y clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN mkdir -p ${THE_HOME}/config \
    && chown app:app ${THE_HOME}/config

ENTRYPOINT ["/tini", "--"]
CMD su - app -c "/usr/bin/deluged -c ${THE_HOME}/config && /usr/bin/deluge-web -c ${THE_HOME}/config"
