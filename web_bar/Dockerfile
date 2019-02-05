FROM certs

FROM nginx

COPY ./default.conf /etc/nginx/conf.d/default.conf
COPY ./nginx.conf /etc/nginx/nginx.conf
RUN mkdir -pv /usr/share/nginx/port80 /usr/share/nginx/port443
ENV SERVER_NAME "bar"
RUN echo "server: ${SERVER_NAME}\nprotocol: http"  >/usr/share/nginx/port80/index.html
RUN echo "server: ${SERVER_NAME}\nprotocol: https"  >/usr/share/nginx/port443/index.html
COPY --from=0 /certs/${SERVER_NAME}.key /etc/ssl/private/${SERVER_NAME}.key
COPY --from=0 /certs/${SERVER_NAME}.pem /etc/ssl/certs/${SERVER_NAME}.pem

