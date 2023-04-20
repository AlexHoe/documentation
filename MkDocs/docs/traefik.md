# traefik
## traefik configuration
### docker-compose.yml
```yaml
version: '3'

services:
  traefik:
    container_name: "traefik"
    image: "traefik:v2.9"
    # The treafik container has to be on the same network as the other containers
    networks:
      - proxy
    # Default ports listens to, port 8080 is the traefik dashboard for inital setup or debugging
    ports:
      - "80:80"
      - "443:443"
        #  - "8080:8080" #Don't do this in production
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock:ro"   # required to notice new docker containers
      - "./letsencrypt:/ssl-certs"  # certificate storage
      - "./config:/etc/traefik"     # separate configuration file for traefik

networks:
  proxy:
    external: true
```
### traefik.yml
```yaml
# Global configuration
global:
  checkNewVersion: true
  sendAnonymousUsage: false

# EntryPoints definition (optional)
entryPoints:
  web:
    address: :80
    # redirect http traffic automatically to https
    http:
      redirections:
        entryPoint:
          to: websecure
          scheme: https

  websecure:
    address: :443

# Enable API and dashboard (optional)
api:
  #  insecure: true # Don't use in production
  dashboard: true # true by default

# Certificates configuration
# Let's Encrypt
certificatesResolvers:
# Staging resolver to test new configurations (not trusted!)
  staging:      
    acme:
      email: alex@hoellerer.one
      storage: /ssl-certs/acme.json
      caServer: "https://acme-staging-v02.api.letsencrypt.org/directory"
      httpChallenge:
        entrypoint: web
# Trusted certificates
  production:
    acme:
      email: alex@hoellerer.one
      storage: /ssl-certs/acme.json
      caServer: "https://acme-v02.api.letsencrypt.org/directory"
      httpChallenge:
        entryPoint: web

# Provider configuration
providers:
  # Enable Docker configuration backend
  docker:
    exposedByDefault: false # Default is true
```
## Client configuration
### docker-compose.yml
```yaml
...
labels:
      - "traefik.enable=true"
      # "cloud" is the name of the router and can be set randomly
      # Host has to be the subdomain for the service
      - "traefik.http.routers.cloud.rule=Host(`cloud.hoellerer.one`)"
      # websecure to use https
      - "traefik.http.routers.cloud.entrypoints=websecure"
      - "traefik.http.routers.cloud.tls=true"
      # resolver can be staging or production
      - "traefik.http.routers.cloud.tls.certresolver=production"
    # the container has to be in the same network as the traefik container
    networks:
      - proxy
...
```
