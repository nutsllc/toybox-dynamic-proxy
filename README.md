# Dynamic proxy engine on Docker

A Dockerfile for the Dynamic proxy engine using Docker container.

This image is registered to the [Docker Hub](https://hub.docker.com/r/nutsllc/toybox-dynamic-proxy/) which is the official docker image registory.

## Docker Compose example

```
proxy-nginx:
    image: nutsllc/toybox-nginx:1.11
    volumes:
        - "/etc/localtime:/etc/localtime:ro"
        - "./.data/nginx/conf.d:/etc/nginx/conf.d"
        - "./.data/nginx/vhost.d:/etc/nginx/vhost.d"
        - "./.data/nginx/docroot:/usr/share/nginx/html"
        - "./.data/nginx/certs:/etc/nginx/certs"
    environment:
        - TOYBOX_UID=1000
        - TOYBOX_GID=1000
    ports:
        - "80:80"
        - "443:443"

proxy-engine:
    image: nutsllc/toybox-dynamic-proxy
    links:
        - proxy-nginx
    volumes_from:
        - proxy-nginx
    volumes:
        - "/etc/localtime:/etc/localtime:ro"
        - "/var/run/docker.sock:/tmp/docker.sock:ro"
    command: -config /docker-gen.conf
```

## Contributing

We'd love for you to contribute to this container. You can request new features by creating an [issue](https://github.com/nutsllc/toybox-dynamic-proxy/issues), or submit a [pull request](https://github.com/nutsllc/toybox-dynamic-proxy/pulls) with your contribution.