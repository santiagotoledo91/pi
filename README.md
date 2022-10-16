### Add a new domain

Let's assume we will use the following values:

- Public domain name: `exampledomain.com`
- Local container hostname:  `exampledomain_com-nginx`

> **IMPORTANT**: The local container must be up and running or nginx will fail to start

Create a new file under `nginx/conf.d` named as `exampledomain_com.conf` with this contents:

```
server {
    server_name exampledomain.com www.exampledomain.com;
    server_tokens off;

    location / {
        proxy_pass          http://exampledomain_com-nginx;
        proxy_set_header    Host $host;
        proxy_set_header    X-Real-IP $remote_addr;
        proxy_set_header    X-Forwarded-Proto https;
        proxy_set_header    X-Forwarded-Host $host;
    }
}
```

To generate the **SSL certificate**, run the following command

```
docker-compose exec nginx bash -c "certbot --nginx --non-interactive --agree-tos -m mail@exampledomain.com -d exampledomain.com -d www.exampledomain.com"
```
