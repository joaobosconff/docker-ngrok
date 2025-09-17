# Ngrok Docker Image 

This is a fork of the [shkoliar/ngrok](https://hub.docker.com/r/shkoliar/ngrok) image, adding ARM64, 386 and ARMv6 architecture support.

Thanks shkoliar !

Ngrok version : 3.29.0

## Overview

This Docker image packages [ngrok](https://ngrok.com), a service that exposes local environments to the public internet through secure tunnels. Built on the official [busybox:glibc](https://hub.docker.com/_/busybox) base image, it maintains a minimal footprint by using only essential components.

## Quick Start

### Basic Usage

To expose a web server container (e.g., `dev_web_1` on port 80):

```bash
docker run --rm -it --link dev_web_1 joaobosconff/ngrok http dev_web_1:80
```

The tunnel remains active until terminated with `Ctrl+C`.

### Advanced Configuration

#### Custom Parameters

```bash
docker run --rm -it --link <container> [--net <network>] joaobosconff/ngrok <params> <container>:<port>
```

#### Environment Variables

```bash
docker run --rm -it --link <container> [--net <network>] \
  --env DOMAIN=<container> \
  --env PORT=<port> \
  shkoliar/ngrok
```

### Docker Compose Integration

```yaml
services:
  ngrok:
    image: joaobosconff/ngrok:latest
    ports:
      - "4551:4551"
    links:
      - web
    environment:
      - DOMAIN=web
      - PORT=80
```

Access the ngrok web interface at `http://localhost:4551`

## Configuration Options

### Environment Variables

| Variable    | Options                    | Default   | Description                                        |
|------------|----------------------------|-----------|---------------------------------------------------|
| PROTOCOL   | http, tls, tcp            | http      | Tunnel protocol                                    |
| DOMAIN     | *                         | localhost | Target hostname/container                          |
| PORT       | *                         | 80        | Target port                                        |
| REGION     | us, eu, ap, au, sa, jp, in| us        | Tunnel endpoint region                            |
| HOST_HEADER| *                         | -         | Custom Host header (e.g., `localdev.test`)        |
| BIND_TLS   | true, false               | -         | Restrict to HTTP/HTTPS only                        |
| SUBDOMAIN  | *                         | -         | Custom subdomain                                   |
| AUTH_TOKEN | *                         | -         | Ngrok authentication token                         |
| DEBUG      | true                      | -         | Enable debug logging                               |
| PARAMS     | *                         | -         | Raw ngrok parameters (overrides other variables)   |

For detailed configuration options, see the [ngrok documentation](https://ngrok.com/docs).

## Troubleshooting

If you encounter network errors like:

```bash
docker: Error response from daemon: Cannot link to /dev_web_1, as it does not belong to the default network.
```

Specify the network explicitly:

```bash
docker run --rm -it --link dev_web_1 --net dev_default joaobosconff/ngrok http dev_web_1:80
```

## License

[MIT License](../../blob/master/LICENSE)
