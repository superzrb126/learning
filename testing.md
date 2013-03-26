To test Docker, we have a set of unittests in both $REPO and $REPO/fs. I've been running these tests like so:

    export GOPATH="/path/to/docker/workspace"
    cd $GOPATH/src/github.com/dotcloud/docker/
    sudo -E go test
    cd fs ; sudo -E go test

Another basic functional test is simply running (with or without `/var/lib/docker` in place) the following:

    docker run base echo "hello world"

You should see docker auto-download (if you removed `/var/lib/docker`) the image "base" and echo "hello world" back to you.