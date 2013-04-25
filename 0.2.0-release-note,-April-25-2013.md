Hi everyone,

Exactly one month after our first release, I am very excited to announce Docker version 0.2.0. The primary focus of this release is stability, but it also packs several cool new features.

Before diving in, let me just say that the volume and quality of contributions we've received - bug reports, feedback, advice, pull requests - has blown my mind. I can't thank you enough for your help, people. Now let's make sure Docker lives up to your expectations!


CONTENTS
=========

* 0.2.0 Summary
  * Stability
  * Full interactive mode
  * Community layers
  * Static port allocation
  * Access ports on localhost

* What's next
  * Searchable index
  * Open-source registry
  * Data volumes
  * Remote API
  * Runtime API
  * Build!

* How you can help

* Hack day on May 2


0.2.0 SUMMARY
=============

Stability
------------

Although Docker is not yet production ready, it has been getting more
stable by the day. Leaks have been plugged, embarrassing shortcuts
have been cleaned up - for a total of 45 bug reports closed!

Again - a million thanks to all of you who contributed to the
bugfixing effort on github, irc, the mailing list, twitter and
sometimes even in person :)


Full interactive mode
------------------------------

Those of you who played with the first versions may remember a half-assed interactive shell, with broken terminal controls and obscure warnings. I encourage you to try again. Thanks to Guillaume Charmes, Docker now has full support for pty emulation, which means:

- Run a shell with flawless terminal controls. You can even run vim in there.

- Arbitrary attaching and detaching. Started a long-running command from your shell? Leave it detached and check back on it later

- Multi-client attach. Try attaching to an interactive session with 2 different clients - they will seamlessly share stdin and stout, just like a screen or tmux session.



Community layers
--------------------------

Thanks to backjlack, tianon and shansi, you can now choose between Debian, Ubuntu, Centos, Gentoo, Arch Linux and Busybox as the base layer for building your containers.

We're also starting to see cool examples of backend services being "dockerized":

- Julien Barbier built a full [memcache-as-a-service proof-of-concept](http://memcachedasaservice.com) on top of Docker as a Stanford CS50 project!

- John Costa wrote a [great post on dockerizing Redis](http://www.johnmcostaiii.net/2013/installing-redis-on-docker/).

- Peter Braden's [NodeJS OpenCV bindings](https://github.com/peterbraden/node-opencv) can now be vendored with all dependencies into a single container.

- A [dockerized IRC bouncer](https://github.com/dotcloud/docker/wiki/Public-docker-images#python-app-builder) by yours truly.


You can find more layers on the [community-maintained wiki page](https://github.com/dotcloud/docker/wiki/Public-docker-images). Make sure to contribute yours so I can feature you next time!


Static port allocation
-----------------------------

Since 0.1 docker can allocate random public TCP ports on-demand and
NAT them to your containers. In 0.2, you can now optionally specify
*which* public TCP port should be allocated.

For example, if public traffic is hitting port 80 of your host
directly, you can now control which container receives that trafic. Of
course, only 1 container can own a given public port at any given time
- docker takes care of this synchronization for you.


Access ports on localhost
--------------------------------------

In 0.1, a container's public TCP ports are accessible on all of the host's network interfaces... except the loopback interface, which lead to frustrating errors when trying to connect to localhost from the host itself.

This has been fixed - you can now connect to your container's public ports on localhost from the host itself.



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


Searchable index
-------------------------

@samalba, @dhrp, @kencochrane and @shin- are working on a searchable
index of all community layers, to replace our charming but inefficient
wiki page. Requests and bug reports for the index should be made on
the main docker repository.

Open-source registry
------------------------------

Many of you have asked for a way to host layers in your own private
registry. This will soon be possible with an open-source version of
the public registry.

Data volumes
--------------------

This feature, also known as "the longest pull request thread ever", is
clearly in high demand. See https://github.com/dotcloud/docker/pull/376.

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

This month I am welcoming dotCloud's 2nd core committer, Guillaume Charmes. If you're willing and able, we'd love to groom you to become one too :)


Check out the [contribution guidelines](https://github.com/dotcloud/docker/blob/master/CONTRIBUTING.md) as a start.


HACK DAY ON MAY 2
=================

Save the date! On May 2nd we're hosting our first public Docker hack day at the dotCloud HQ in San Francisco.



Thanks for reading this far. Open-source is awesome.

Happy hacking
Solomon