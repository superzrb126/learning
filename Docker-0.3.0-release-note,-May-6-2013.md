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

Many of you have asked for a way to host layers in your own private registry. We promised this would be possible soon - and now it is! We are very happy to announce that the docker registry is now open-source.

There are 2 main use cases for the open-source registry:

### Use case 1: private registry

You shouldn't have to open-source your code to benefit from Docker - and Docker just isn't as cool without the registry. Now you can deploy your own private registry, and use it to share containers within your organization. For example, if you're building a "private PaaS" you could use the registry to store all application builds, ready to be 'docker pull'-ed from any server in your cluster.

### Use case 2: public mirror

Even for layers shared by the community, there has been concern that the central registry might become a single point of failure. Wouldn't it be nice to have a mirror system, so that the burden of hosting all these layers can be decentralized, minimizing the impact of downtimes?

Now we can! The new registry API from the ground up to support decentralized mirroring scenarios. This means a registry can redirect layer downloads to 3d-party registries *in a secure way*, so that compromised registries cannot inject malicious content.


The new registry is available at https://github.com/dotcloud/docker-registry

## Feedback?

We're always available on irc.freenode.net #docker. Feel free to open an issue on Docker's main repository: https://github.com/dotcloud/docker