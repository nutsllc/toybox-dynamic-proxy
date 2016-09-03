# Dynamic proxy engine on Docker

A Dockerfile for the Dynamic proxy engine using Docker container.

This image is registered to the [Docker Hub](https://hub.docker.com/r/nutsllc/toybox-dynamic-proxy/) which is the official docker image registory.

## Usage

To run dynamic proxy by docker-compose:

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

Then start any containers you want proxied with an environment variable VIRTUAL_HOST=sub.your-domain.com

```
$ docker run -e VIRTUAL_HOST=foo.bar.com  ...
```

The containers being proxied must expose the port to be proxied, either by using the EXPOSE directive in their Dockerfile or by using the --expose flag to docker run or docker create.

Provided your DNS is setup to forward foo.bar.com to the a host running nginx-proxy, the request will be routed to a container with the VIRTUAL_HOST env var set.

For more detiail, see a readme of [jwilder/nginx-proxy](https://github.com/jwilder/nginx-proxy/blob/master/README.md) and [jwilder/docker-gen](https://github.com/jwilder/docker-gen/blob/master/README.md).

## LICENSE

* [jwilder/nginx-proxy](https://github.com/jwilder/docker-gen) MIT
* [jwilder/docker-gen](https://github.com/jwilder/docker-gen) MIT

## Contributing

We'd love for you to contribute to this container. You can request new features by creating an [issue](https://github.com/nutsllc/toybox-dynamic-proxy/issues), or submit a [pull request](https://github.com/nutsllc/toybox-dynamic-proxy/pulls) with your contribution.