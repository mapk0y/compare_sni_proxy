FROM nginx

COPY stream.conf /etc/nginx/stream.d/stream.conf
RUN echo 'include /etc/nginx/stream.d/*.conf;' >> /etc/nginx/nginx.conf
