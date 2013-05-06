Hi everyone,

Today we're excited to announce a new version of Docker. This version brings three highly demanded features: data volumes, a searchable index and the first open-source release of the docker registry!

Let us know what you think! We're always available on irc.freenode.net #docker. Feel free to open an issue on Docker's main repository: https://github.com/dotcloud/docker.


# CONTENTS

* What is Docker?
* 0.3.0 summary
  * Data volumes
  * Searchable index
  * Open-source registry
* What's next?
  * Remote API
  * Runtime API
  * Build!
* How you can help
* Hack day on June 11


# What is Docker?

Docker is an open-source engine which automates the deployment of applications as highly portable, self-sufficient containers.

Docker containers are both hardware-agnostic and platform-agnostic. This means that they can run anywhere, from your laptop to the largest EC2 compute instance and everything in between - and they don't require that you use a particular language, framework or packaging system. That makes them great building blocks for deploying and scaling web apps, databases and backend services without depending on a particular stack or provider.

Docker is an open-source implementation of the deployment engine which powers [dotCloud](http://www.dotcloud.com), a popular Platform-as-a-Service. It benefits directly from the experience accumulated over several years of large-scale operation and support of hundreds of thousands of applications and databases.


# 0.3.0 summary

## Data volumes

You can now mark directories within a container as holding persistent data. This will cause the runtime to handle them differently. Specifically:

* Changes to a data volume are not recorded as regular filesystem changes in the top layer, and will not be included at the next commit.

* Changes to a data volume are made directly, without the overhead of a copy-on-write mechanism. This is good for very large files.

* Most importantly, **data volumes can be shared and reused between containers**. This is the feature that makes data volumes so powerful. You can use it for anything from hot database upgrades to custom backup or replication tools. See [the documentation](http://docs.docker.io/en/latest/examples/couchdb_data_volumes/) for an example.

Using data volumes is as simple as adding a new flag:

```bash
docker run -v /var/lib/couchdb -v /var/log shykes/couchdb
```

## Searchable index

We are upgrading the central registry with a new searchable index, available at http://index.docker.io.

You can use the index to search for ready-to-use containers uploaded by the community - everything from [Redis](https://index.docker.io/search?q=redis) to [Memcache](https://index.docker.io/search?q=memcached), [Hipache](https://index.docker.io/search?q=hipache) or [OpenCV](https://index.docker.io/search?q=opencv). You can also browse [popular containers](https://index.docker.io/search?s=popular) or simply [manage your own uploads](https://index.docker.io/account/login/).

Make sure you install Docker 0.3 to take advantage of it!

## Open-source Registry

Many of you have asked for a way to host layers in your own private registry. We promised this would be possible soon - and now it is! We are very happy to announce that the docker registry is now open-source.

There are 2 main use cases for the open-source registry:

### Use case 1: private registry

You shouldn't have to open-source your code to benefit from Docker - and Docker just isn't as cool without the registry. Now you can deploy your own private registry, and use it to share containers within your organization. For example, if you're building a "private PaaS" you could use the registry to store all application builds, ready to be 'docker pull'-ed from any server in your cluster.

### Use case 2: public mirror

Even for layers shared by the community, there has been concern that the central registry might become a single point of failure. Wouldn't it be nice to have a mirror system, so that the burden of hosting all these layers can be decentralized, minimizing the impact of downtimes?

Now we can! The new registry API from the ground up to support decentralized mirroring scenarios. This means a registry can redirect layer downloads to 3d-party registries *in a secure way*, so that compromised registries cannot inject malicious content.


The new registry is available at https://github.com/dotcloud/docker-registry


WHAT'S NEXT?
============

Expect Docker to improve rapidly in several areas! Docker is more than
a side-project for dotCloud - we are investing significant engineering
resources in it, and all future dotCloud products will be based on it.

At the same time, I am making sure the contribution process is 100%
open. That means everything is discussed and decided in the open, and
the rules for contributing are the same regardless of your employer.
I'm looking forward to welcoming our first non-dotCloud core committer
:)


Here are a few points where you should expect rapid progress:


Remote API
------------------

The whole point of Docker is to be a building block for complex
automation. We have made the CLI as scriptable as possible, but at
some point you just need a stable API for remote programmatic access.
See https://github.com/dotcloud/docker/issues/21

Runtime API
------------------

There is a lot of interest in customizing *how* docker containers are
executed, especially in production environments where many technical
choices have already been made: storage, networking, distro, kernel
version, logging, access control... I can't implement all the
customizations you requested, but I can freeze a runtime API which
will let everyone contribute extensions while preserving the
portability of containers.

Build!
--------

So it turns out docker is an great build tool. As a developer I can
define precisely, step-by-step, how to build my application and all
its dependencies into a single container which can run anywhere. No
more dependency hell, no more "works on my machine". I like to call
that methodology "extreme vendoring", and it's pure awesome. Today it
[still requires hacky contrib tools](https://github.com/dotcloud/docker/tree/v0.2.0/contrib/docker-build)
 but we're going to pull it into docker.


HOW YOU CAN HELP
================

Feel like contributing? Awesome! There are several ways you can help.


Become a documentation maintainer
------------------------------------------------------

We're looking for volunteers to contribute maintain sub-sections of
the documentation, especially install instructions, to make sure
they're always up-to-date. Ping me if you're interested.


Make screencasts and articles
--------------------------------------------

If you do anything cool and useful with docker, record a screencast
and tell us about it! This could be dockerizing an application,
installing it in a specific environment, cool usage tricks, etc. We
recommend ascii.io, it's insanely easy to use.


Integrate your favorite tools
----------------------------------------

Docker plays well with other tools in the devops toolbox. There are
already efforts to integrate with Vagrant, Chef, Puppet, Openstack,
Mesos... Got a tool you want to integrate with Docker? Create a github
issue and we'll help you out.


Dockerize your app
----------------------------

It's still rough, but if you want to experiment with "extreme
vendoring" of your application, get in touch and we'll help you out.
Take a look at https://github.com/peterbraden/node-opencv for an
example.

Join the conversation
-------------------------------

There are insane volumes of interesting conversations going on on irc
(#docker@freenode), twitter (#docker) and [the google group](https://groups.google.com/forum/?fromgroups#!forum/docker-club).

Whether you have a beginner question or want to discuss a point of
design, never hesitate to speak up!

And of course all the usual ways of spreading the word - tweets,
github follows, etc. etc. are always welcome.


Become a contributor
-------------------------------

Got time on your hands and want to write some Go? We love pull
requests, and will gladly help you find your way through the internals
and guide you through your first pull request.


Check out the [contribution guidelines](https://github.com/dotcloud/docker/blob/master/CONTRIBUTING.md) as a start.


HACK DAY ON JUNE 11
=================

The next Docker hack is on June 11 at the dotCloud HQ in San Francisco. RSVP now at http://www.meetup.com/Docker-meetups/events/118026552/



Happy hacking!

Solomon & the Docker maintainers