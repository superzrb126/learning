### Background

```docker run``` is meant to run docker containers as if they were UNIX processes. ```docker run``` doesn't behave just like UNIX processes because it doesn't return the exit code of the process running in the container and it doesn't have support for temporary containers.

A proof of concept implementation which wraps docker run can be found at [Dockrun at github.com](https://github.com/unclejack/dockrun).

### Purpose

The goal of these changes is to make ```docker run``` easy to monitor with systemd, upstart, supervisor and other process monitoring solutions and systems.
These changes would make ```docker run``` standalone redundant. Standalone mode wouldn't be needed any more because monitoring the docker daemon is enough and monitoring is going to be used for ```docker run``` anyway, one more process to monitor won't cause any problems.

### Proposed changes

The proposed changes are separated to maximize their usefulness to make them useful in more scenarios.
```docker run``` needs the following features in order to have a more UNIX process like behavior:

**"temporary" flag**

This flag would make ```docker run``` delete the container when the process exits. This flag could be ```-r``` because it's not currently used by ```docker run```.

**"exit code" flag**

This flag would make ```docker run``` return the exit code of the process running in the container, instead of returning 0 when the container process exits.
The flag used for this could be ```-s```.
Note: ```docker run``` will have to return the exit code of the process running in the container even if a signal to terminate is received by ```docker run```.