# A beginners guide to Docker

19 Aug 2014 [Vincent Batts (Red Hat)](https://opensource.com/users/vbatts) [Feed](https://opensource.com/user/17306/feed) 564[up](https://opensource.com/business/14/7/guide-docker?rate=jUbGGx8XBljB4O-PSPgAUiG5gAQQh1J7eKjTFokRVj4)



It doesn't matter whether it is pianos or teddy bears, containers provide consistency that manufacturers and transporters can rely on. Just as shipping containers have revolutionized the import/export industry, you've heard that Docker is doing the same in tech.

It is catching like wild fire because of its relatable and recognizable use cases, and it's easy enough for anybody to use.

## Understanding Docker

Containers are a way of providing the same consistency that dev and ops can rely on. Docker uses containers as a way of isolating one or more processes. Processes (PIDs) on a host need memory, CPU, network access, and disk access. There is a subtle environment that the application needs in order to run. This includes specific executables, specific libraries, and a specific standard C library (libc). Obviously, a kernel is behind a lot of these components, but as far as the PIDs are concerned these resources are often taken for granted, and access to the kernel is abstract via libc.

There are a number of ways to set up environments for PIDs. If you need an alternate version of libc, a change root (chroot) is a basic option. If there is a need to constrain memory, kernel control groups (cgroups) is an increasingly feature-rich option. A virtual machine (VM) can be the answer, but this introduces a hypervisor to encapsulate the entirety of the contained runtime, and the need of an entire initialization/boot for the machine to be usable. If the PID needed is only for a job or a task, VMs can be a resource expensive route. To have a change root feeling, with cgroups, isolation, and the root filesystem easily shareable, turn to Docker. It achieves all of that while also providing an programmatic way to create new root filesystems!

The developers at [Dotcloud, Inc.](https://www.dotcloud.com/) began work on Docker in January 2013 with a public release in March the same year. Prior to creating Docker, Dotcloud has been a platform-as-a-service (PaaS), so a tool like Docker which allows building a PaaS environment anywhere is very logical next step for them. Docker's GitHub page reflects the frenzied excitement since its public release. In just one year, the project has gained 500+ contributors with less than 10 being employees of Docker. More than 70 releases, 13,000+ stars, and 2,000+ forks is a phenomenal amount of activity for an infant open source project. Just in time for the first inaugural [DockerCon](http://www.dockercon.com/), in June 2014, Docker released version 1.0.0, boasting stability and production-readiness for running linux-amd64 containers.

Docker's audience is developers and sysadmins, and focuses on meeting the challenges they face. There are problems so familiar to developers and sysadmins that they fade into everyday routine.

Woes of development can be:

- keeping a consistent environment while iterating through new builds
- documenting so others can use your code like you already know how to
- installing dependent software without conflicting with host versions
- consistent testing environment
- accountable data and traceable storage

Woes of operations can be:

- proper documentation of the software being monitored
- process micro-management and resource quotas
- network configuration for application requirements
- accountable data and traceable storage

By having a standardized image format, development sees all servers the same and operations sees all containers the same. These are the woes that Docker directly addresses. It is first necessary to distinguish the use of "image" and "container" within the Docker environment.

### An image

Portable: They can be pushed to a registry, or saved as a tar archive.

Layered: The steps in producing an image, are added in layers. In this way, images that are mostly the same, except for the last few steps, can reduce disk usage by sharing parent layers.

Static: The contents are not changeable, unless making a new image.

### A container

Runtime: An environment for PIDs.

Writable: It is essentially an ephemeral storage.

Layered: It is on an image.

These terms will show up in various contexts, and it is important to see how they relate to each other, but are also their own entities. With this foundation it's time to explore its basic Docker application and capabilities.

## Docker 101

> ```bash
> $> docker run busybox echo 'Hello, World!' Hello, World!
> ```

Let's unpack this simple example. First, the command is docker, and telling docker to ``run`` the 'busybox' image. If the image 'busybox' is not present, then docker will attempt to fetch an image named 'busybox' from the public [Docker hub](https://hub.docker.com/). Then Docker sets up the layers of this image, all the cgroups and namespaces for this container environment, and executes ``echo 'Hello, World!'``.

> ```bash
> $> docker pull fedora
> ```

This is a direct way to fetch an image named 'fedora' from the public Docker hub.

> ```bash
> $> docker images
> ```

This shows the images available locally. There are many more docker commands like these. To see them all, see ``docker help``. It is possible to run a docker image interactively and decide later to commit that container as an image for future use.

> ```bash
> $> $ docker run fedora touch file
> $> docker ps -l
> CONTAINER ID  IMAGE      COMMAND     CREATED  STATUS          NAMES
> 9626763a35f9  fedora:20  touch file  6s ago   Exited (0) 3s   prickly_goldstine
> $> docker commit 9626763a35f9 my-touched-file
> $> docker run my-touched-file ls -s /file
> 0 /file
> ```

For a programmatic way to commit new images, there is the "Dockerfile" format. A Dockerfile is the file with the steps to prepare an image. On first glance it looks like a shell script with a little metadata.

> ```dockerfile
> FROM fedora
> RUN yum install -y mongodb-server && mkdir -p /data/db
> EXPOSE 27017
> VOLUME ["/data/db"]
> CMD mongod
> ```

We're building on the 'fedora' image, running a shell command to install software and make directory, then declaring a port to publish, and directory to store in. Finally, we're declaring the command to be executed for this container's runtime (e.g. 'mongod').

With these instructions in a file named "Dockerfile", run:

> ```bash
> $> docker build -t mongodb .
> ```

All the output of the steps are visible and once it finishes successfully a 'mongodb' image is available.

Running service container Using this new 'mongodb' image built with the Dockerfile, fire it up and review it.

> ```bash
> $> docker run -it -p 127.0.0.1:27017:27017 -v $(pwd)/db:/data/db mongodb
> ```

The mongodb server will start and preallocate files needed.

Here is an explanation of the new flags since the "Hello, World!" example:

- ``-it`` connects the terminal, so that <ctrl>+c can kill the attached process.
- ``-p 127.0.0.1:27017:27017`` attaches port 27017 inside the container to 127.0.0.1:27017 on the host
- ``-v $(pwd)/db:/data/db`` mounts the '/data/db' volume inside the container, to $(pwd)/db on the host.

Now with the mongodb container running, in a separate terminal run the following:

> ```bash
> $> docker ps
> CONTAINER ID  IMAGE          COMMAND             CREATED  
> cb5346e10a69  mongodb:latest /bin/sh -c mongod   2min ago
> 
> STATUS        PORTS                    NAMES
> Up 2min   127.0.0.1:27017->27017/tcp   hopeful_torvalds    
> ```

The runtime information of the container running on our 'mongodb' image appears. To end this process, either <ctrl>+c the original, or ``docker kill cb5346e10a69`` that container. Since the only data written by that ephemeral container, was in the /data/db directory, which is mounted to the host directory, that same container can be restarted and see that all the pre-allocated data is reused, even though this new ``docker run`` is itself a clean-slate.

### Local sharing

In addition to aforementioned public registry of docker images, Docker hub, there is also the [Docker registry](https://github.com/dotcloud/docker-registry), which allows hosting of local Docker images. While many Linux distributions have packaged the docker-registry, it is easier to use docker itself to run a Docker registry.

> ```bash
> $> docker run -d -p 5000:5000 -v $(pwd)/registry-data:/tmp registry:latest
> ```

Now we have a local service that Docker images can be pushed to and pull from. Since the service is listening on "0.0.0.0" of the host, images need to be tagged with the host's IP address, or DNS name. It is recommended to use the DNS name, so these is no need to re-tag the local docker images for a new IP address.

To use this local registry, images need to include it in the image name. This can be set at the time of ``docker build`` or can be tagged as such. Assuming a DNS name of "image-storage.local.lan" (this is completely arbitrary) pointing to the host's IP address, run:

> ```bash
> $> docker tag mongodb image-storage.local.lan:5000/mongodb
> $> docker push image-storage.local.lan:5000/mongodb
> ```

Now others on the local network are able to pull and run this image by referencing this new tagged name.

### Consistent Developer Environment

Many of the Docker use cases are around production services and scalability, but an even more relatable use case is the developer's environment. Availability of Docker images can create the environment needed for legacy code, current code, and bleeding edge code. Now there is no need for a VM-hypervisor, or to be on the network to access "the build machine with the right versions of everything". Further, once these development images are prepared and shared on a local registry, then any machine able to can run an image and becomes a possible workstation.

Like the layering mentioned earlier, the following Dockerfile can be split to make a common-base-image, then allows builds for individual usernames for the last steps.

> ```dockerfile
> FROM fedora:20
> 
> # setup and install common tools
> RUN yum groupinstall -y "Development tools"
> RUN yum install -y vim-enhanced git
> CMD bash -l
> 
> # this part could be per-user
> RUN useradd -m -u 1000 vbatts
> ENV HOME /home/vbatts
> WORKDIR /home/vbatts
> USER vbatts
> ```

Build this, and tag it for the local registry:

> ```bash
> $> docker build -t image-storage.local.lan:5000/$USER/devel:f20 .
> [...]
> $> docker push image-storage.local.lan:5000/$USER/devel:f20
> ```

Now to use this build. This development image allows the host to keep sources and home directory as is, while grafting this standardized development image around the active shell.

> ```bash
> function devel() {
>         docker run \
>         --rm \
>         -it \
>         --hostname=$(hostname)-devel \
>         -v $HOME:$HOME \
>         -v $SSH_AUTH_SOCK:$SSH_AUTH_SOCK \
>         --env SSH_AUTH_SOCK \
>         --env PATH=/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:/sbin \
>         --workdir $(pwd) \
>         ${1+"$@"} \
>         image-storage.local.lan:5000/$USER/devel:f20
> }
> ```

This is a bash function for ``~/.bashrc``. This defines a function called ``devel``, that will enter a bash shell of the development image just built.

Here is a walk through of all the flags passed to ``docker run``:

- ``--rm`` this piece of housekeeping removes the container after the shell exits
- ``-it`` makes an interactive and attached terminal
- ``--hostname=$(hostname)-devel`` sets the hostname inside the container, but appends 'devel' to help distinguish
- ``-v $HOME:$HOME`` mounts current home directory to the same inside the container
- ``-v $SSH_AUTH_SOCK:$SSH_AUTH_SOCK`` mounts an active ssh agent socket inside the container
- ``--env SSH_AUTH_SOCK`` pass in the ssh agent environment variable
- ``--env PATH=...`` cleans the PATH slate
- ``--workdir $(pwd)`` starts the bash shell in the same directory as current working directory (e.g. ~/src/my/big/project/)
- ``${1+"$@"}`` makes additional flags to ``devel`` land here in the function

> ```bash
> vbatts@noyee ~ $> cd src/my/big/project
> vbatts@noyee ~/src/my/big/project
> $> devel
> vbatts@noyee-devel ~/src/my/big/project $> make
> [...]
> ```

This keeps a seamless experience without worrying of messing up or cluttering the host with tools and libraries only needed for specific projects.

### CI / Testing

Using continuous integration (CI) can provide an engine for building and testing of software. Building a Dockerfile can wrap this into a single action, leaving a usable image if it succeeds.

> ```bash
> [...]
> RUN cd /src/myapp && git pull --force origin master && \
>  autoreconf && \
>  ./configure && \
>  make
> RUN cd /src/myapp && \
>  make test
> ```

If anything in the Dockerfile exits non-zero, building the image fails. Further, by having a Dockerfile build from a base image, and by being able to customize its own environment, build requirements on a shared build host, conflicting versions, and any number of complications are alleviated.

Through these examples, many everyday use cases can be recognized. It is not difficult to see why this technology is igniting a fire in the development-to-production life cycle. While being a low-level building block, it is simultaneously revolutionizing the orchestration of computing environments. Join the conversation and help cultivate the container ecosystem that Docker is growing.