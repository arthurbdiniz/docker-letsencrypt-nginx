# Letsencrypt Docker 

## How to run 

1) Replace example.org with your domain name and the server ip/port

```bash
# data/nginx/app.conf
upstream whoami {
    server 10.0.0.10:8000;
}

server_name example.org;

ssl_certificate /etc/letsencrypt/live/example.org/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/example.org/privkey.pem;
```

```sh
# init-letsencryptinit-letsencrypt
domains=(example.org)
```

2) Create a dummy certificate

```bash
chmod +x init-letsencrypt.sh
sudo ./init-letsencrypt.sh
```

3) Automatic Certificate Renewal
To activate auto renew uncoment the following lines on `docker-compose.yml` file:

Nginx service
```yml

# command: "/bin/sh -c 'while :; do sleep 6h & wait $${!}; nginx -s reload; done & nginx -g \"daemon off;\"'"

```

Certbot service
```yml

# entrypoint: "/bin/sh -c 'trap exit TERM; while :; do certbot renew; sleep 12h & wait $${!}; done;'"

```


4) Everything is in place now. The initial certificates have been obtained and our containers are ready to launch. Simply run docker-compose up and enjoy your HTTPS-secured website or app.
```bash
docker-compose up
```