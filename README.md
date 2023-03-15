# Docker image for [Cloudflared](https://github.com/cloudflare/cloudflared), a proxy-dns service.
_

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

## Index
__

* [Build locally](#build-locally)
* [Image](#image)
* [Environment variables](#environment-variables)
* [Ports](#ports)
* [Usage](#usage)
  * [Docker Compose](#docker-compose)
  * [Command line](#command-line)
* [Notes](#notes)
  * [Use with Pi-hole](#use-with-pi-hole)
  * [Upgrade](#upgrade)

## Build locally

```shell
git clone https://github.com/frittenlab/docker-cloudflared.git
cd docker-cloudflared

# Build image and output to docker (default)
docker buildx bake

# Build multi-platform image
docker buildx bake image-all
```

## Image

| Registry                                                                                         | Image                           |
|--------------------------------------------------------------------------------------------------|---------------------------------|
| [Docker Hub](https://hub.docker.com/r/frittenbude/cloudflared/)                                          | `frittenbude/cloudflared`                 |
| [GitHub Container Registry](https://github.com/users/frittenlab/packages/container/package/cloudflared)  | `ghcr.io/frittenlab/cloudflared`        |

Following platforms for this image are available:

```
$ docker run --rm mplatform/mquery frittenbude/cloudflared:latest
Image: frittenbude/cloudflared:latest
 * Manifest List: Yes
 * Supported platforms:
   - linux/amd64
   - linux/arm/v6
   - linux/arm/v7
   - linux/arm64
```

## Environment variables

* `TZ`: The timezone assigned to the container (default `UTC`)
* `TUNNEL_DNS_UPSTREAM`: Upstream endpoint URL, you can specify multiple endpoints for redundancy. (default `https://1.1.1.1/dns-query,https://1.0.0.1/dns-query`)
* `TUNNEL_DNS_PORT`: DNS listening port (default `5053`)
* `TUNNEL_DNS_ADDRESS`: DNS listening IP (default `0.0.0.0` "all interfaces")
* `TUNNEL_METRICS`: Prometheus metrics host and port. (default `0.0.0.0:49312`)

## Ports

* `5053/udp`: Listen port for the DNS over HTTPS proxy server
* `49312/tcp`: Listen port for metrics reporting

## Usage

### Docker Compose

Docker compose is the recommended way to run this image. You can use the following [docker compose template](examples/compose/docker-compose.yml), then run the container:

```bash
docker-compose up -d
docker-compose logs -f
```

### Command line

You can also use the following minimal command :

```bash
docker run -d --name cloudflared \
  -p 5053:5053/udp \
  -p 49312:49312 \
  frittenbude/cloudflared:latest
```

## Notes

### Use with Pi-hole

[Pi-hole](https://pi-hole.net/) currently [provides documentation](https://docs.pi-hole.net/guides/dns-over-https/) to manually set up DNS-Over-HTTPS with Cloudflared.

With Docker and this image, it's quite easy to use it with [Pi-hole](https://pi-hole.net/). Take a look at this simple [docker compose template](examples/pihole/docker-compose.yml) and you're ready to go.

## Upgrade

To upgrade, pull the newer image and launch the container :

```bash
docker-compose pull
docker-compose up -d
```
