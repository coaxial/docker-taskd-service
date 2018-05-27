FROM coaxial/s6-base

COPY root/ /

VOLUME /ssl_certs/ /client_certs/
EXPOSE 53589

ENTRYPOINT ["/init"]
