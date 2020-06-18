# A static site using Nginx and Docker (and Kubernetes)

Docker Hub repo https://hub.docker.com/repository/docker/chudinov/nginx-hello

## Docker
This repo contains code for building a simple static website served using an Nginx container inside Docker. The code for the site is contained in `index.html`, and the Nginx config is in `default.conf`. The Dockerfile contains commands to build a Docker Image.

### Build
To build a Docker image from the Dockerfile, run the **docker build** command from inside this directory:

```sh
$ docker build -t <docker-hub-username>/nginx-hello:<version> .
```
version is default "latest".

Example:
```sh
$ docker build -t nginx-hello .
```

This will produce the following output

```sh
Sending build context to Docker daemon 81.41 kB
Step 1/3 : FROM nginx:alpine
 ---> 2f3c6710d8f2
Step 2/3 : COPY default.conf /etc/nginx/conf.d/default.conf
 ---> Using cache
 ---> 176c56cc07b6
Step 3/3 : COPY index.html /usr/share/nginx/html/index.html
 ---> 3407953dafd0
Removing intermediate container cb64bb3e3aca
Successfully built 3407953dafd0
Successfully tagged nginx-hello:latest
```
### Run
To run the image in a Docker container, use the **docker run** command:
```sh
$ docker run -itd --name mycontainer --publish 8080:80 <docker-hub-username>/nginx-hello:<version>
```
Port **80** is the port in Docker. Port **8080** is the local port.

Example:
```sh
$ docker run -itd --name mycontainer --publish 8080:80 nginx-hello
```
This will start serving the static site on port 8080. If you visit `http://localhost:8080` in your browser, you should be able to see our static site.

### Tag
To differ between versions of the same image we **tag** it with the **docker tag** command:
```sh
$ docker tag nginx-hello <docker-hub-username>/nginx-hello:<version>
```
Example:
```sh
$ docker tag nginx-hello chudinov/nginx-hello:1.0
```

### Push
To push the image to a registry, use the **docker push** command:
```sh
$ docker push <docker-hub-username>/nginx-hello:<version>
```

Example:
```sh
$ docker push chudinov/nginx-hello:1.0
```
## Kubernetes