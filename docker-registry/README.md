# Docker Registry: Vagrant VM #

This box provisions a Virtualbox VM containing a Docker Registry.

**Warning!** This is an insecure registry for *development use and POCs only.*

## Prerequisites (Windows)

Windows 7 or 10 x64, with at least 4 GB RAM, and:

1. Vagrant 1.8.6 or higher.
1. Virtualbox 5.0 or higher.
1. Hardware Virtualization enabled in BIOS.

## How to run

In a terminal, execute the following:
1. `git clone` this repository.
1. `cd` to this directory.
1. Execute `vagrant up`.

### What does this do?

1. Creates a Virtualbox VM (based on Ubuntu 14)
1. Installs Docker and creates a Docker Registry
1. Opens up ports as needed

## Registry Info

Registry URL:
```
84.84.87.88:5000
```

1. A registry UI is not created. (This is a `TODO` for me)
1. You can query the list of images in the registry using the registry API. See the notes below.

## Notes

### Testing that registry works

Push an image to this registry.

```
# pull an image from the Docker Hub.
docker pull hello-world:latest

# retag it.
docker tag hello-world 84.84.87.88:5000/hello-world

# Push it to your local Docker Registry
docker push 84.84.87.88:5000/hello-world
```

Then query the registry to verify that it exists.

```
vagrant@ubuntu-14:~$ curl -X GET http://84.84.87.88:5000/v2/_catalog
{"repositories":["hello-world"]}
```


### Interacting with the registry

*Get the list of images in this Docker Registry:*

```
curl -X GET http://84.84.87.88:5000/v2/_catalog
{"repositories":[]}
```

### Connecting to this registry
If you want to pull images from this registry, your Docker Daemon will need to be modified.

**Example for Ubuntu 14.04**
On Ubuntu 14.04, the following additional parameter needs to be added to `/etc/default/docker`.

```
# Use DOCKER_OPTS to modify the daemon startup options.
DOCKER_OPTS=" ... ... ... --insecure-registry=84.84.87.88:5000"
```
This points the Docker daemon to use the registry in insecure mode.


### References

1. [Using Docker Registry in insecure mode](https://docs.docker.com/registry/insecure/)