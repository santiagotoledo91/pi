FROM nginx:1.23.1-alpine

RUN apk add certbot certbot-nginx bash vim

COPY conf.d/* /etc/nginx/conf.d/
