# Traefik v2 HTTPS (SSL)

Traefik setup for Docker Environment with SSL enabled.

To get start, clone this repo:
```
git clone https://github.com/tb2010/traefik-docker-ssl.git
```


Next, go to the root of the repo 
```bash
cd traefik-docker-ssl`
```

Edit domain, network and any desired customization settings in docker-composer.yml and ./traefik/config.yml


generate a local wildcard certificate using openssl or mkcert (google for documentation)

- if you have mkcert installed (macos: brew install mkcert; mkcert --install) you can generate a cert ie:
```bash
mkcert -cert-file certs/local-cert.pem -key-file certs/local-key.pem "docker.localhost" "*.docker.localhost"
```

Create netowrks thar will be used by traefik
```bash
docker network create dev-proxy
```

Now, start the container with:
```bash
docker-compose -f docker-compose.yml up -d
```

For any services that you would like to expose via the traefik proxy, you need to add the following labels
```
- "traefik.enable=true"
- "traefik.http.router.<servicename>.rule="Host(`<servicename>.docker.localhost`)"
- "traefik.http.router.<servicename>.tls=true"
# if port is different than 80, add the following servier port label
#- "traefik.http.services.<servicename>.loadbalancer.server.port=<port>"
```

