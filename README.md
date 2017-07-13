# nginx Docker Image

## Supported tags and respective `Dockerfile` links

- [`1.13.3`, `mainline`, `1`,  `1.13`, `latest` (*mainline/Dockerfile*)](https://github.com/MatthewVance/nginx-docker/tree/master/mainline)

## What is nginx?

Nginx (pronounced "engine x") is a web server. It can act as a reverse proxy server for TCP, UDP, HTTP, HTTPS, SMTP, POP3, and IMAP protocols, as well as a load balancer and an HTTP cache.
> [wikipedia.org/wiki/Nginx](https://en.wikipedia.org/wiki/Nginx)

## How to use this image

### Standard usage

Run this container with the following command:

```console
docker run --name my-nginx -d -p 80:80 \
--restart=always mvance/nginx:latest
```

Read the official nginx Docker image documentation for more ideas.

# User feedback

## Documentation

Documentation for this image is stored right here in the [`README.md`](https://github.com/MatthewVance/nginx-docker/blob/master/README.md).

Documentation for nginx is available on the [project's website](https://nginx.org/en/docs/).

## Issues

If you have any problems with or questions about this image, please contact me through a [GitHub issue](https://github.com/MatthewVance/nginx-docker/issues).

## Contributing

You are invited to contribute new features, fixes, or updates, large or small. 

Please familiarize yourself with the [repository's `README.md` file](https://github.com/MatthewVance/nginx-docker/blob/master/README.md) before attempting a pull request.

Before you start to code, I recommend discussing your plans through a [GitHub issue](https://github.com/MatthewVance/nginx-docker/issues), especially for more ambitious contributions. This gives other contributors a chance to point you in the right direction, give you feedback on your design, and help you find out if someone else is working on the same thing.

## Acknowledgments

The code in this image was significantly influenced by:

- [nginx alpine Dockerfile](https://github.com/nginxinc/docker-nginx/blob/master/mainline/alpine/Dockerfile)
- [Wonderfall nginx Dockerfile](https://github.com/Wonderfall/dockerfiles/blob/master/nginx/Dockerfile)
- [build_nginx.sh](https://gist.github.com/MattWilcox/402e2e8aa2e1c132ee24) from [@MattWilcox](https://github.com/MattWilcox)


## Licenses

### License

Unless otherwise specified, all code is released under the MIT License (MIT). See the [repository's `LICENSE` file](https://github.com/MatthewVance/nginx-docker/blob/master/LICENSE) for details.

### Licenses for other components

- nginx Alpine Docker image: [2-clause BSD License](https://github.com/nginxinc/docker-nginx/blob/master/LICENSE)
- Wonderfall nginx Dockerfile: [CC0 1.0 Universal](https://github.com/Wonderfall/dockerfiles/blob/master/LICENSE)
