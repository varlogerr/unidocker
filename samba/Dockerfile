FROM debian:9.3
LABEL maintainer="Varlog <varlog.err@gmail.com>"

ENV TINI_VERSION=v0.16.1
ENV THE_USER=app
ENV THE_GROUP=app
ENV THE_HOME=/home/app
ENV SHARE_DIR=/app

ARG THE_USER_ID
ARG THE_GROUP_ID

ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
ADD ["tmp", "/tmp/samba"]

RUN chmod +x /tini

RUN addgroup -q --gid $THE_GROUP_ID $THE_GROUP \
    && adduser -q --disabled-login \
      --ingroup $THE_GROUP \
      --shell /bin/bash --gecos ",,," \
      --home $THE_HOME \
      --uid $THE_USER_ID $THE_USER

RUN apt-get update \
    && apt-get install -y samba \
    && apt-get clean

RUN cat /tmp/samba/templates/my_smbd.conf \
      | sed -e "s#{{ share_dir }}#$SHARE_DIR#" \
      | sed -e "s#{{ the_user }}#$THE_USER#" \
      | sed -e "s#{{ the_group }}#$THE_GROUP#" \
      | tee /etc/samba/my_smbd.conf \
    && echo "include = /etc/samba/my_smbd.conf" >> /etc/samba/smb.conf

RUN if [ -z ${SAMBA_PASS+x} ]; then (echo $SAMBA_PASS; sleep 1; echo $SAMBA_PASS) | smbpasswd -s -a app; fi

RUN rm -rf /tmp/samba

ENTRYPOINT ["/tini", "--"]
CMD smbd -FS < /dev/null
