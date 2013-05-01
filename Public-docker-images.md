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


### Ubuntu 12.04 LTS Precise base image

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
docker pull tianon/gentoo:latest
```

Note that this image is somewhat limited, being that it is simply a faithful copy of the stage3 tarball (see also [#313](https://github.com/dotcloud/docker/issues/313#issuecomment-15883754)).  Including a copy of the portage tree would increase the image size by an appreciable amount, and would become stale quickly.

### Arch Linux base image

(Available somewhere, can't remember where)

### Debian base images

#### Standard images

##### Squeeze (stable)
```
docker pull tianon/debian:squeeze
```
or
```
docker pull tianon/debian:6.0.7
```

##### Wheezy (testing)
```
docker pull tianon/debian:wheezy
```
or
```
docker pull tianon/debian:7.0
```

##### Sid (unstable)
```
docker pull tianon/debian:sid
```

#### Rolling release images

##### Stable
```
docker pull tianon/debian-roll:stable
```
or
```
docker pull tianon/debian-roll:6.0.7
```

##### Testing
```
docker pull tianon/debian-roll:testing
```
or
```
docker pull tianon/debian-roll:7.0
```

##### Unstable
```
docker pull tianon/debian-roll:unstable
```

### Redis

http://redis.io

See @johncosta 's [awesome blog post](http://www.johnmcostaiii.net/2013/installing-redis-on-docker/).

```
docker run -p 6379 johncosta/redis redis-server
```

### Apache CouchDB

[Couchdb](http://github.com/apache/couchdb) is "an open source database that focuses on ease of use and on being a database that completely embraces the web".

```
docker run -d -p 5984 shykes/couchdb /bin/sh -e /usr/bin/couchdb -a /etc/couchdb/default.ini -a /etc/couchdb/local.ini -b -r 5 -p /var/run/couchdb/couchdb.pid -o /dev/null -e /dev/null -R
```

Container source available at http://github.com/shykes/couchdb


### PostgreSQL

http://www.postgresql.org/

```
docker run -p 5432 jpetazzo/pgsql /init YourSecretPassword
```
This will provision a PostgreSQL container, and create a `root` user with password `YourSecretPassword`.

Container source available at https://gist.github.com/jpetazzo/5494158


### Hipache

https://github.com/dotcloud/hipache

```
docker run -p :6379 -p :80 samalba/hipache supervisord -n
```

This will launch a supervisord in foreground which spawns an Hipache daemon (using the dev config, but easy to change) + redis-server.

The redis-server is spawned inside the same container on purpose. Current Hipache's architecture does not share a Redis among several machines, but among several Hipache's workers in the same machine.

Container source is available in the Hipache's repos: https://github.com/dotcloud/hipache/blob/master/Dockerfile

### Firefox/VNC

```
docker run -d -p 5900 creack/firefox-vnc x11vnc -forever -usepw -create
```

This will launch a VNC server and when a client connects to it, it starts Firefox.
The default password is `1234`

Container source available at http://github.com/creack/docker-firefox/


### Memcached

http://memcached.org/

See this [article](http://www.slideshare.net/julienbarbier42/memcached-as-a-service-using-docker).

```
docker run -p 11211 jbarbier/memcached memcached -u daemon
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