FROM certs

FROM alpine:3.8

ENV GOSS_VERSION 0.3.6
RUN wget https://github.com/aelsabbahy/goss/releases/download/v${GOSS_VERSION}/goss-linux-amd64 -O /bin/goss \
    && chmod 755 /bin/goss

WORKDIR /work

COPY --from=0 /certs/bar.pem /etc/ssl/certs/bar.pem
COPY --from=0 /certs/foo.pem /etc/ssl/certs/foo.pem

CMD ["goss", "serve"]
