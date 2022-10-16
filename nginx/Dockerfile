FROM nginx:1.23.1-alpine

RUN apk add certbot certbot-nginx bash vim

# Config for: ronitascrafts.com
COPY conf.d/ronitascrafts_com.conf /etc/nginx/conf.d/ronitascrafts_com.conf
RUN certbot --nginx --non-interactive --agree-tos -m santiagotoledo91@gmail.com -d ronitascrafts.com -d www.ronitascrafts.com
