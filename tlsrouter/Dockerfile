FROM golang:1.11
RUN CGO_ENABLED=0 GOOS=linux go get -a -installsuffix cgo go.universe.tf/tcpproxy/cmd/tlsrouter

FROM alpine:latest
COPY --from=0 /go/bin/tlsrouter .
COPY ./tlsrouter.conf /tlsrouter.conf
CMD ["/tlsrouter", "-conf", "/tlsrouter.conf"]

