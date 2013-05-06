Hi everyone,

we're excited to announce the new version of Docker today. This version is really special since it brings support for the public Index allowing repository search.

# 0.3.0 summary

## New searchable index

We are upgrading the central registry with a new searchable index, available at http://index.docker.io.

You can use the index to search for ready-to-use containers uploaded by the community - everything from [Redis](https://index.docker.io/search?q=redis) to [Memcache](https://index.docker.io/search?q=memcached), [Hipache](https://index.docker.io/search?q=hipache) or [OpenCV](https://index.docker.io/search?q=opencv).

You can also browse [popular containers](https://index.docker.io/search?s=popular) or simply [manage your own uploads](https://index.docker.io/account/login/).

Make sure you install Docker 0.3 to take advantage of it!

## Open-source Registry

The Index references repositories that are hosted on Registry servers. We are hosting the main one. But we also decided to open-source the Registry source code to allow anyone to control the hosting of his own Registry server. Storing images on a 3rd party Registry still allows to be referenced in the official Index.

The source code is available at: https://github.com/dotcloud/docker-registry

## Feedback?

We're always available on irc.freenode.net #docker. Feel free to open an issue on Docker's main repository: https://github.com/dotcloud/docker