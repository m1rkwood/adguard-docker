# Adguard setup

This docker will setup adguard on your server.  
To use this configuration, you will have to also run the `traefik-docker` project available [here](https://github.com/m1rkwood/traefik-docker).  
You will also need to combine it with [Wireguard](https://github.com/m1rkwood/wireguard).

## Folder structure

```
.env
docker-compose.yml
```

## Envionment

Duplicate the `.env.template` file and rename it to `.env`  
Fill the information:

```
TZ=
VIRTUAL_HOST=
```

`TZ` is your timezone, i.e. `America/Los Angeles`.  
`VIRTUAL_HOST` is the complete url (with subdomain) where the adguard dashboard will appear.  

## First Run
For the first run, Adguard will need to start on port 3000 instead of 80.
In order to do this, you will have to modify the `docker-compose.yml` file in the `traefik-docker` project by adding the port `3000`:
```
ports:
  - "80:80"
  - "443:443"
  - "3000:3000"
```
Restart Traefik
```
docker-compose up -d
```
Then modify this line in the `docker-compose.yml` file of the `adguard-docker` project (replace `80` by `3000`)
```
- "traefik.http.services.adguard.loadbalancer.server.port=3000"
```
Start the containers
```
docker-compose up -d
```
Then go to the address that you specified as `VIRTUAL_HOST` and complete the Adguard setup in the interface (Leave AdGuard default configuration on port 80). Set a user and password (important).

Once it's finished, change back the `docker-compose.yml` files in the `traefik-docker` and `adguard-docker` projects (remove the changes we did in the previous steps). Restart the containers for both projects (`docker-compose up -d`), you should now be able to access Adguard at the address you specified as `VIRTUAL_HOST`.
