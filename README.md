### Setup

- Start pihole (to create the network)
```bash
$ cd ~/pi && docker-compose up -d pihole
```
- Start any needed backend:
```bash
$ cd ~/ronitascrafts_com && docker-compose up -d
```
- Start nginx
```bash
$ cd ~/pi && docker-compose up -d nginx
```
- Create the certificates:
```bash
$ cd ~/pi && docker-compose exec nginx bash -c "certbot --nginx --non-interactive --agree-tos -m santiagotoledo91@gmail.com -d ronitascrafts.com -d www.ronitascrafts.com"
```

### Add a new domain

Let's assume we will use the following values:
```
Public domain name: `exampledomain.com`
Local container hostname:  `exampledomain_com-nginx`
```
> **IMPORTANT**: The local container must be up and running or nginx will fail to start

- Create a new file under `nginx/conf.d` named as `exampledomain_com.conf` with this contents:

```
server {
    server_name exampledomain.com www.exampledomain.com;
    server_tokens off;
    
    resolver 127.0.0.11 ipv6=off;
    set $backend "http://exampledomain_com-nginx";

    location / {
        proxy_pass          $backend;
        proxy_set_header    Host $host;
        proxy_set_header    X-Real-IP $remote_addr;
        proxy_set_header    X-Forwarded-Proto https;
        proxy_set_header    X-Forwarded-Host $host;
    }
}
```

- To generate the **SSL certificate**, run the following command:

```bash
$ docker-compose exec nginx bash -c "certbot --nginx --non-interactive --agree-tos -m mail@exampledomain.com -d exampledomain.com -d www.exampledomain.com"
```

- To apply any configuration change, rebuild the container
```bash
$ docker-compose up -d --build nginx
```
