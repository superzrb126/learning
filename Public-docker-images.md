# Public docker images

You can find a more complete list of images at [index.docker.io](http://index.docker.io)

## Searching the index

You can use `docker search` to search [index.docker.io](http://index.docker.io) for images from the command line.

```
docker search debian
Found 6 results matching your query ("debian")
NAME                               DESCRIPTION
tianon/debian                      
tianon/debian-roll                 
mzdaniel/debian                    
wdtz/debian-6.0-x86                Debian 6.0 (x86), based on OpenVZ template...
findspire/wheezy                   Template image of Debian Wheezy.
rockstack/rockstack-debian-build   
```

Followed by docker pull to get an image.

`docker pull tianon/debian`

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

```
docker pull base/arch
```

### openSUSE Linux base image

```
docker pull flavio/openSUSE_12.3
```

### Debian base images

#### Standard images

##### Wheezy (stable)
```
docker pull tianon/debian:wheezy
```
or
```
docker pull tianon/debian:7.1
```

##### Jessie (testing)
```
docker pull tianon/debian:jessie
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
docker pull tianon/debian-roll:7.1
```

##### Testing
```
docker pull tianon/debian-roll:testing
```

##### Unstable
```
docker pull tianon/debian-roll:unstable
```

### Redis

http://redis.io

See @johncosta 's [awesome blog post](http://www.johnmcostaiii.net/2013/installing-redis-on-docker/).

```
docker run johncosta/redis
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

```bash
BUILD_JOB=$(docker run -d shykes/pybuilder buildapp http://github.com/shykes/helloflask/archive/master.tar.gz)
docker wait $BUILD_JOB
BUILD_IMAGE=$(docker commit $BUILD_JOB)
docker run -p 5000 $BUILD_IMAGE runapp
```

### NodeJS + OpenCV

A ready-to-use build of the [OpenCV library](http://opencv.org) including [NodeJS 0.8 bindings](https://github.com/peterbraden/node-opencv/).

```bash
docker run shykes/node-opencv node -e 'console.log(require("opencv").version)'
```

### [Logbot](https://github.com/dannvix/Logbot)

IRC bot, with two flavors of real-time web interface: [Full-screen](http://logbot.g0v.tw/) and [embedded widget](http://g0v.tw/).

```bash
# Instant IRC bot + Web UI
docker run -e 'LOGBOT_NICK=bot_name_here' \
           -e 'LOGBOT_CHANNELS=#docker,#g0v.tw' \
           -e 'LOGBOT_SERVER=irc.freenode.net' \
       audreyt/logbot
```

### [EtherCalc](http://ethercalc.net/) (with Redis + Node.js + WebWorker-Threads)

A collaborative spreadsheet editor. This is the same docker image powers our public site at http://ethercalc.org/.

```bash
# Runs at port 6967 (default)
docker run audreyt/ethercalc

# Runs at another port, for example 8080
docker run -p 8080:6967 audreyt/ethercalc
```

### [Discourse](http://www.discourse.org/) (with Redis + Postgresql + Sidekiq + Nginx)

A modern forum engine. Instructions to run the five containers at https://github.com/srid/discourse-docker
