# docker-powerdns

PowerDNS + Recursor + Admin GUI + Adblock in one single Docker

## Configuration options

See [Dockerfile](Dockerfile#L9)

## IPv6 support

In order to enable this Docker to handle IPv6 queries, you should do some further actions. See this [preparation script](https://github.com/julianxhokaxhiu/vps-powered-by-docker/blob/master/modules/dns_server.sh#L16) for more informations.

## Ad-Block feature

If you want to enable ad-blocking on top of your entries, just set the [relative environment variable](Dockerfile#L27) to `true`. List courtesy of [Pi-Hole project](https://pi-hole.net/).

The list will be updated using cron, at the time specified on the [relative environment variable](Dockerfile#L24).

## How to use

### Simple

```
docker run \
    --restart=always \
    -d \
    --name=pdns \
    -p 53:53 \
    -p 53:53/udp \
    -p 8080:8080 \
    -p 8081:8081 \
    -v "/docker/pdns:/srv/data" \
    -t aasaidane/powerdns
```

### Advanced

```
docker run \
    --restart=always \
    -d \
    --name=pdns \
    -e "CUSTOM_DNS=8.8.8.8;8.8.4.4;[2001:4860:4860::8888];[2001:4860:4860::8844]" \
    -e "API_KEY=my-awesome-api-key" \
    -e "SOA_NAME=my-custom-domain" \
    -e "CRONTAB_TIME=0 10 * * *" \
    -e "ENABLE_ADBLOCK=true" \
    -p 53:53 \
    -p 53:53/udp \
    -p 8080:8080 \
    -p 8081:8081 \
    -v "/docker/pdns:/srv/data" \
    -t aasaidane/powerdns
```


## DNSSEC option

To manage DNSSEC, you must connect to the container.
```
docker exec -ti pdns bash
```

### Show domain secure zones

```
pdnsutil show-zone example.com
```

### Secure domain

```
pdnsutil secure-zone example.com
```
