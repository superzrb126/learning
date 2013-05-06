Hi everyone,

we're excited to announce the new version of Docker today. This version is really special since it brings support for the public Index allowing repository search.

# 0.3.0 summary

## Data volumes

You can now mark directories within a container as holding persistent data. This will cause the runtime to handle them differently. Specifically:

* Changes to a data volume are not recorded as regular filesystem changes in the top layer, and will not be included at the next commit.

* Changes to a data volume are made directly, without the overhead of a copy-on-write mechanism. This is good for very large files.

* Most importantly, data volumes can be shared and reused between containers. This is the feature that makes data volumes so powerful. You can use it for anything from hot database upgrades to custom backup or replication tools. See [the documentation](http://docs.docker.io/en/latest/examples/couchdb_data_volumes/) for an example.

Using data volumes is as simple as adding a new flag:

```bash
docker run -v /var/lib/couchdb -v /var/log shykes/couchdb
```

## Searchable index

We are upgrading the central registry with a new searchable index, available at http://index.docker.io.

You can use the index to search for ready-to-use containers uploaded by the community - everything from [Redis](https://index.docker.io/search?q=redis) to [Memcache](https://index.docker.io/search?q=memcached), [Hipache](https://index.docker.io/search?q=hipache) or [OpenCV](https://index.docker.io/search?q=opencv).

You can also browse [popular containers](https://index.docker.io/search?s=popular) or simply [manage your own uploads](https://index.docker.io/account/login/).

Make sure you install Docker 0.3 to take advantage of it!

## Open-source Registry

The Index references repositories that are hosted on Registry servers. We are hosting the main one. But we also decided to open-source the Registry source code to allow anyone to control the hosting of his own Registry server. Storing images on a 3rd party Registry still allows to be referenced in the official Index.

The source code is available at: https://github.com/dotcloud/docker-registry

## Feedback?

We're always available on irc.freenode.net #docker. Feel free to open an issue on Docker's main repository: https://github.com/dotcloud/docker