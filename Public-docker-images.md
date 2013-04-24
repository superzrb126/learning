# Public docker images

## Official images

### All images from the Ubuntu repository
(try this if you get a 404 error for an individual Ubuntu image)

```
docker pull ubuntu
```

### Ubuntu 12.10 Quantal base image

```
docker pull ubuntu:12.10
```


### Ubuntu 12.04 TLS Precise base image

```
docker pull ubuntu:12.04
```


### Centos 6.4 base image

Thanks to @backjlack

```
docker pull centos
```

### Busybox base image


```
docker pull busybox
```


## Unofficial images

### Gentoo base image

```
docker pull tianon/gentoo
```

Note that this image is somewhat limited, being that it is simply a faithful copy of the stage3 tarball (see also [#313](https://github.com/dotcloud/docker/issues/313#issuecomment-15883754)).  Including a copy of the portage tree would increase the image size by an appreciable amount, and would become stale quickly.

### Arch Linux base image

(Available somewhere, can't remember where)

### Debian Squeeze base image

Not available, maintainer wanted!

### Debian Wheezy base image

```
docker pull tianon/debian:wheezy
```

### Redis

http://redis.io

See @johncosta 's [awesome blog post](http://www.johnmcostaiii.net/2013/installing-redis-on-docker/).

```
docker run -p 6379 johncosta/redis redis-server
```

### ZNC irc bouncer

```
docker run -p 6667 -u irc shykes/znc zncrun
```

Source code available at http://github.com/shykes/docker-znc

### Dockerbuilder

Build and upload binary releases of docker

```
docker run shykes/dockerbuilder dockerbuilder REVISION S3_ID S3_KEY
```

### Python app builder

Build a python web app. Source code is at http://github.com/shykes/pybuilder

```
BUILD_JOB=$(docker run -d shykes/pybuilder buildapp http://github.com/shykes/helloflask/archive/master.tar.gz)
docker wait $BUILD_JOB
BUILD_IMAGE=$(docker commit $BUILD_JOB)
docker run -p 5000 $BUILD_IMAGE runapp
```

### NodeJS + OpenCV

A ready-to-use build of the [OpenCV library](http://opencv.org) including [NodeJS 0.8 bindings](https://github.com/peterbraden/node-opencv/).

```
docker run shykes/node-opencv node -e 'console.log(require("opencv").version)'
```
