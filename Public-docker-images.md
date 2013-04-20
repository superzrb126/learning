# Public docker images

## Official images

### Ubuntu 12.10 Quantal base image

```
docker pull ubuntu:12.10
```


### Ubuntu 12.04 TLS Precise base image

```
docker pull ubuntu:12.04
```


### Centos 6.4 base image

```
docker pull centos
```


## Unofficial images

### Gentoo base image

(Available somewhere, can't remember where)

### Arch Linux base image

(Available somewhere, can't remember where)

### Debian Squeeze base image

Not available, maintainer wanted!


### Redis

http://redis.io

```
docker run -p 6379 shykes/redis redis-server
```

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

