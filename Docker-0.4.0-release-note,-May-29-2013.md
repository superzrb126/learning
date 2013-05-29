Dear Dockers,

Today I am happy to introduce a new release of Docker. In addition to numerous stability and usability fixes, this release introduces 2 highly anticipated features: Remote API and Build.

# Contents

* What is Docker?
* 0.4.0 summary
  * Remote API
  * Build
  * Reliability improvements
* What's next?
  * Broader kernel support
  * Better documentation
  * Plugin API
  * Production-ready
  * Play nice with other tools
* How you can help
* Hack day on June 11


# What is Docker?


[Docker](http://docker.io) is an open-source engine which automates the deployment of applications as highly portable, self-sufficient containers.

Docker containers are both hardware-agnostic and platform-agnostic. This means that they can run anywhere, from your laptop to the largest EC2 compute instance and everything in between - and they don't require that you use a particular language, framework or packaging system. That makes them great building blocks for deploying and scaling web apps, databases and backend services without depending on a particular stack or provider.

Docker is an open-source implementation of the deployment engine which powers [dotCloud](http://www.dotcloud.com), a popular Platform-as-a-Service. It benefits directly from the experience accumulated over several years of large-scale operation and support of hundreds of thousands of applications and databases.

# 0.4.0 summary

## Remote API

The whole point of Docker is to be a building block for complex automation. We have made the CLI as scriptable as possible, but at some point many of you found it difficult to write code that interacts with docker... until now.

Docker can now be remotely controlled via a simple HTTP/JSON protocol. This remote API is properly versioned and [fully documented](http://docs.docker.io/en/latest/api/docker_remote_api.html), so you can use it to integrate your favorite software project with Docker. It is also used by the official client, which means it supports all docker features.

In addition to the reference implementation, the community has already started working on client libraries for [Go](https://github.com/dotcloud/docker/pull/640), [Python](https://github.com/dotcloud/docker-py) and [Ruby](https://github.com/ActiveState/docker-ruby). Want to implement a client for your favorite language? [Get in touch!](https://github.com/dotcloud/docker/issues/new?title=Remote%20API%20client%20in%20MY_FAVORITE_LANGUAGE&body=Hi%2C%0aI'm+interested+in+implementing+a+client+library+for+the+docker+remote+API.+Is+anyone+else+interested+in+doing+this+with+me%3F%0a%0aI'm+also+not+sure+why+the+Docker+team+took+the+time+to+urlencode+this+ridiculously+long+message+in+their+link,+especially+considering+I'm+about+to+erase+the+whole+thing.+But+hey%2C+thanks+anyway,+I+guess!)

We look forward to discovering the amazing things you'll build with this!

# Build

Docker now features a *build* command, which can automatically build containers from your application's source code. All you need to do is add a simple *Dockerfile*.

A *Dockerfile* is a very simple text file - usually no longer than 5 to 10 lines - which expresses all your application's dependencies in one place and streamlines the process of assembling them - complete from base system to language-specific packages. *docker build* uses this file to assemble a new container, layer by layer.

For example, here's the Dockerfile [of the Hipache project](https://github.com/dotcloud/hipache/blob/master/Dockerfile):

```
from	ubuntu:12.04
run	echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
run	apt-get -y update
run	apt-get -y install wget git redis-server supervisor
run	wget -O - http://nodejs.org/dist/v0.8.23/node-v0.8.23-linux-x64.tar.gz | tar -C /usr/local/ --strip-components=1 -zxv
run	npm install hipache -g
run	mkdir -p /var/log/supervisor
copy	supervisord.conf	/etc/supervisor/conf.d/supervisord.conf
```

And here's how you can build it:

```
$ git clone http://github.com/dotcloud/hipache
$ cd hipache
$ docker build .
```

## Reliability improvements

# What's next?
  ## Broader kernel support
  ## Better documentation
  ## Plugin API
  ## Production-ready
  ## Play nice with other tools
# How you can help
# Hack day on June 11
