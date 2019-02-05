FROM alpine:3.8

RUN apk add --no-cache openssl
ENV SUBJECT "/C=JP/ST=Kanagawa/L=Yokohama/O=mohi/OU=Web"
WORKDIR /certs
RUN openssl req -new -x509 \
      -keyout bar.key \
      -out bar.pem \
      -days 3650 \
      -nodes \
      -subj "${SUBJECT}/CN=*.bar.example.com" \
    && chmod -v 600 bar.key
RUN openssl req -new -x509 \
      -keyout foo.key \
      -out foo.pem \
      -days 3650 \
      -nodes \
      -subj "${SUBJECT}/CN=*.foo.example.com" \
    && chmod -v 600 foo.key
