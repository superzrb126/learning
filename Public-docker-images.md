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


