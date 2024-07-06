## Assign domains

### Configure domain provider

1. Visit your domain-provider (e.g. `domains.google.com`)
1. Go to `DNS` -> `Default name servers` -> `Custom records` -> `Manage custom records`
1. create record as
   - Host name: name of subdomain, e.g. `whoami` for `whoami.example.com`, keep empty if you are going to use main domain like `example.com`
   - Type: `A`
   - Data or Target: ip-address of your server/vps, e.g. `76.76.21.21`
   - Keep value of `TTL` as it is

### Assigning a Domain to a Container

- [Reference](https://doc.traefik.io/traefik/user-guides/docker-compose/basic-example/)

1. Visit the subdomain or domain you want to assign to the container and make sure it is not currently in use, e.g. `whoami.example.com` (`whoami.<domain-name>`)

1. Create a directory (e.g. `traefik`) and create `docker-compose.yml` with following code in inside it

   ```yml
   version: "3.3"

   services:
     traefik:
       image: "traefik:v2.10"
       container_name: "traefik"
       command:
         #- "--log.level=DEBUG"
         - "--api.insecure=true"
         - "--providers.docker=true"
         - "--providers.docker.exposedbydefault=false"
         - "--entrypoints.web.address=:80"
       ports:
         - "80:80"
         - "8080:8080"
       volumes:
         - "/var/run/docker.sock:/var/run/docker.sock:ro"

     whoami:
       image: "traefik/whoami"
       container_name: "simple-service"
       labels:
         - "traefik.enable=true"
         - "traefik.http.routers.whoami.rule=Host(`whoami.localhost`)"
         - "traefik.http.routers.whoami.entrypoints=web"
   ```

1. replace `whoami.localhost` with your domain you want to assign to container, e.g. `whoami.example.com`
1. run `docker compose up -d`
1. now visit again to the domain (`whoami.example.com`), and you will see domain is now working

### Assigning HTTPS to a Domain

- [Reference](https://doc.traefik.io/traefik/user-guides/docker-compose/acme-tls/#docker-compose-with-lets-encrypt-tls-challenge)

1. Create a directory (e.g. `traefik`) and create `docker-compose.yml` with following code in inside it

   ```yml
   version: "3.3"

   services:
     traefik:
       image: "traefik:v2.10"
       container_name: "traefik"
       command:
         #- "--log.level=DEBUG"
         - "--api.insecure=true"
         - "--providers.docker=true"
         - "--providers.docker.exposedbydefault=false"
         - "--entrypoints.websecure.address=:443"
         - "--certificatesresolvers.myresolver.acme.tlschallenge=true"
         #- "--certificatesresolvers.myresolver.acme.caserver=https://acme-staging-v02.api.letsencrypt.org/directory"
         - "--certificatesresolvers.myresolver.acme.email=postmaster@example.com"
         - "--certificatesresolvers.myresolver.acme.storage=/letsencrypt/acme.json"
       ports:
         - "443:443"
         - "8080:8080"
       volumes:
         - "./letsencrypt:/letsencrypt"
         - "/var/run/docker.sock:/var/run/docker.sock:ro"

     whoami:
       image: "traefik/whoami"
       container_name: "simple-service"
       labels:
         - "traefik.enable=true"
         - "traefik.http.routers.whoami.rule=Host(`whoami.example.com`)"
         - "traefik.http.routers.whoami.entrypoints=websecure"
         - "traefik.http.routers.whoami.tls.certresolver=myresolver"
   ```

1. Execute `docker compose down` if traefik service is running already, and then `docker compose up -d`
1. you can now visit your domain at `https://<domain-name>`, e.g. `https://whoami.example.com`
1. The only thing we need to update for future containers is `label` property with 4 labels
   1. `traefik.enable=true`
   1. `traefik.http.routers.whoami.rule=Host(`whoami.example.com`)`
   1. `traefik.http.routers.whoami.entrypoints=websecure`
   1. `traefik.http.routers.whoami.tls.certresolver=myresolver`
1. The only things to change to assign domain for another container is:
   1. name `whoami` in `routers.whoami`, as every router will have different name (you can choose whatever name you like)
   1. domain-name inside `Host(`whoami.example.com`)`

### Assigning HTTPS to a Domain to already running container

1. create `labels` file with labels list in it
   ```
   test.enable=true
   test.http.routers.whoami.entrypoints=websecure
   test.http.routers.whoami.rule=Host(`whoami.work1.in`)
   test.http.routers.whoami.tls.certresolver=myresolver
   ```
1. run `docker run --label-file ./labels <container-name>`

## [Concepts](https://doc.traefik.io/traefik/getting-started/concepts/)

1. EntryPoints: define the port which will receive the packets, and whether listen to TCP or UDP
2. Routers: connecting incoming requests to the services that can handle them
3. Middlewares: attached to the routers, modify the requests or responses before they are sent to services
4. Services: configuring how to reach the actual services

### [Providers](https://doc.traefik.io/traefik/providers/overview/)

- Configuration discovery in Traefik is achieved through Providers.
- The providers are infrastructure components, whether orchestrators, container engines, cloud providers, or key-value stores.
- Four catcategories of providers:
  1. Label-based: each deployed container has a set of labels attached to it
  2. Key-Value-based: each deployed container updates a key-value store with relevant information
  3. Annotation-based: a separate object, with annotations, defines the characteristics of the container
  4. File-based: uses files to define configuration
