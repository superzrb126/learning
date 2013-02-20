## Quick Install

Assuming root on Ubuntu 12.10 you can just run:

    curl -Ls http://j.mp/docker-quickstart | bash

As you can see by inspecting the URL, this will:

1. Use apt-get to install LXC
1. Download the latest binary
1. Install client and server into /usr/local/bin
1. Create simple Upstart script if one does not exist
1. Start the server
1. Pull in public base image (Ubuntu Quantal) if no image named "base"