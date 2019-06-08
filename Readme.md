# Caddy for linux arm7 arm64 & amd64

A [Docker](http://docker.com) image for [Caddy](http://caddyserver.com) that will let you to serve http or https with Let's Encrypt certificate autogenerated. This image includes the [git](http://caddyserver.com/docs/git) plugin. Plugins can be configured via the `PLUGIN` file at build time. Forked from [abiosoft/caddy-docker](https://github.com/abiosoft/caddy-docker/) initially to provide an arm compatible container, now also compatible with amd64 architecture (thanks to its [Multi-Arch](https://blog.docker.com/2017/11/multi-arch-all-the-things/) base image)

## Details
- [Source Repository](https://github.com/DeftWork/caddy-docker)
- [Deft.Work my personal blog](http://deft.work)

| Docker Hub | Docker Pulls | Docker Stars | Size/Layers |
| --- | --- | --- | --- |
| [caddy-docker](https://hub.docker.com/r/elswork/arm-caddy "elswork/arm-caddy on Docker Hub") | [![](https://img.shields.io/docker/pulls/elswork/arm-caddy.svg)](https://hub.docker.com/r/elswork/arm-caddy "caddy-docker on Docker Hub") | [![](https://img.shields.io/docker/stars/elswork/arm-caddy.svg)](https://hub.docker.com/r/elswork/arm-caddy "caddy-docker on Docker Hub") | [![](https://images.microbadger.com/badges/image/elswork/arm-caddy.svg)](https://microbadger.com/images/elswork/arm-caddy "caddy-docker on microbadger.com") |

[![dockeri.co](https://dockeri.co/image/elswork/arm-caddy)](https://hub.docker.com/r/elswork/arm-caddy)

## Latest Enhancements
- Add support to arm64
- Upgrade Caddy version to 1.0.0

## Getting Started

This is a simple usage for testing proposes.

```sh
$ docker run -d -p 2015:2015 elswork/arm-caddy:latest
```

Point your browser to `http://127.0.0.1:2015`.

## My Real Usage Example

In order everyone could take full advantages of the usage of this docker container, I'll describe my own real usage setup.
```sh
$ docker run -d \
    --name myarm-caddy \
    -p 80:80 -p 443:443 \
    -v /var/www/html:/srv \
    -v $HOME/Caddyfile/https:/etc/Caddyfile \
    -v $HOME/.caddy:/root/.caddy \
    elswork/arm-caddy:latest
```
`--name myarm-caddy` This is absolutely optional, it helps to me to easily identify and operate the container after the first execution.
```sh
$ docker start myarm-caddy
$ docker stop myarm-caddy
$ docker rm myarm-caddy
...
```
`-p 80:80 -p 443:443` Caddy will serve https by default.

`-v /var/www/html:/srv` /var/www/html is the local folder where I have the files to serve. I mount in the container folder that Caddy use to serve files.

`-v $HOME/Caddyfile/https:/etc/Caddyfile` $HOME/Caddyfile/https is my Caddy configuration file. 
```
mydomain.com, www.mydomain.com {
browse
gzip
log stdout
errors stdout
tls user@host.com
}
```

`-v $HOME/.caddy:/root/.caddy` I mount the folder where I will store the Let's Encrypt certificates that will be generated automatically the first time the container run, in order to prevent regenerating them each time.

## Let's Encrypt Auto SSL
**Note** that this does not work on local environments.

You must use a valid domain properly configured to point your docker server and add email to your Caddyfile to avoid prompt at runtime.
