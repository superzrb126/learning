Hi everyone,

we're excited to announce the new version of Docker today. This version is really special since it brings support for the public Index allowing repository search.

# Release Content

## Public Index

The public Index is available at https://index.docker.io/. You can create and manage your user account. You can also search for public repository. The portal features are pretty basic for the moment but it'll be the base for much more.

The use of the Index requires the last version of Docker (> 0.3.x) to be installed.

## Registry

The Index references repositories that are hosted on Registry servers. We are hosting the main one. But we also decided to open-source the Registry source code to allow anyone to control the hosting of his own Registry server. Storing images on a 3rd party Registry still allows to be referenced in the official Index.

The source code is available at: https://github.com/dotcloud/docker-registry

## Feedback?

We're always available on irc.freenode.net #docker. Feel free to open an issue on Docker's main repository: https://github.com/dotcloud/docker