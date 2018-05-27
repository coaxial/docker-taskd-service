FROM coaxial/s6-base

RUN apk --no-cache add taskd

ENV TASKDDATA=/var/taskd
VOLUME /var/taskd

COPY root/ /

VOLUME /ssl_certs/ /client_certs/
EXPOSE 53589

ENTRYPOINT ["/init"]
