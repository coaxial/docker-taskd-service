FROM certbot/certbot

VOLUME /ssl_certs
ENV LE_EMAIL=admin@example.org

# install s6
ARG S6_OVERLAY_VERSION=1.21.4.0
ARG S6_OVERLAY_ARCH=x86
ENV S6_LOGGING_SCRIPT ""

ADD https://github.com/just-containers/s6-overlay/releases/download/v$S6_OVERLAY_VERSION/s6-overlay-$S6_OVERLAY_ARCH.tar.gz /tmp

RUN tar xzf /tmp/s6-overlay-$S6_OVERLAY_ARCH.tar.gz -C / &&\
rm /tmp/s6-overlay-$S6_OVERLAY_ARCH.tar.gz

# install snooze
ADD https://api.github.com/repos/chneukirchen/snooze/tarball/master /tmp
ARG SNOOZE_DEPS="make gcc musl-dev"
RUN mkdir /tmp/snooze &&\
  tar xzf /tmp/master -C /tmp/snooze --strip 1 &&\
  cd /tmp/snooze &&\
  apk --no-cache add $SNOOZE_DEPS &&\
  make install &&\
  rm -rf /tmp/snooze &&\
  rm -rf /tmp/master &&\
  apk --no-cache del $SNOOZE_DEPS

COPY root/ /

ENTRYPOINT ["/init"]
