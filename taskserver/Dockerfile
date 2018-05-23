FROM coaxial/taskserver

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

# install fswatch
ARG FSWATCH_VERSION=1.11.3
ARG FSWATCH_DEPS="make gcc musl-dev"
ADD https://github.com/emcrisostomo/fswatch/releases/download/1.11.3/fswatch-$FSWATCH_VERSION.tar.gz /tmp
RUN mkdir /tmp/fswatch &&\
  tar xzf fswatch-$FSWATCH_VERSION.tar.gz -C /tmp/fswatch &&\
  cd /tmp/fswatch &&\
  apk --no-cache add $FSWATCH_DEPS &&\
  ./configure && make && make install &&\
  rm -rf /tmp/fswatch &&\
  rm -rf /tmp/fswatch-$FSWATCH_VERSION.tar.gz &&\
  apk --no-cache del $FSWATCH_DEPS

COPY root/ /

ENTRYPOINT ["/init"]
