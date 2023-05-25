# Persistent Data

When a container is started created with `docker create` or `docker run`, a read/writable filesystem layer is added on top of it, which allows us to read or write data to a container.

When the container stops and restarted again, the data persists. But, **not** when a container is removed and even built from the same image again. As image is only a template for building containers and the ufs read write layer is removed when we remove a container.&#x20;

{% embed url="https://docs.docker.com/storage/" %}

Also, user data or databases should be separated from application binaries. Docker gives us features to ensure these "separation of concerns". This data is known as **persistent data**.

There are two ways to do this :&#x20;

* Volumes : Make special location outside of container UFS. Volumes are stored in `/var/lib/docker/volumes/` and managed by docker. These shouldn't be modified by other processes.
* Bind Mounts : Link container path to host path. They can be stored anywhere and may be changed by other processes.

<figure><img src="https://docs.docker.com/storage/images/types-of-mounts.png" alt="" width="563"><figcaption></figcaption></figure>

**Volumes**&#x20;

> You can create a volume explicitly using the `docker volume create` command, or Docker can create a volume during container or service creation.
>
> When you create a volume, it is stored within a directory on the Docker host. When you mount the volume into a container, this directory is what is mounted into the container. This is similar to the way that bind mounts work, except that volumes are managed by Docker and are isolated from the core functionality of the host machine.
>
> A given volume can be mounted into multiple containers simultaneously. When no running container is using a volume, the volume is still available to Docker and is not removed automatically. You can remove unused volumes using `docker volume prune`.
>
> When you mount a volume, it may be **named** or **anonymous**. Anonymous volumes are not given an explicit name when they are first mounted into a container, so Docker gives them a random name that is guaranteed to be unique within a given Docker host. Besides the name, named and anonymous volumes behave in the same ways.
>
> Volumes also support the use of _volume drivers_, which allow you to store your data on remote hosts or cloud providers, among other possibilities.
>
> Good use cases for volumes : \
> \
> \- Sharing data among multiple running containers. If you don’t explicitly create it, a volume is created the first time it is mounted into a container. When that container stops or is removed, the volume still exists. Multiple containers can mount the same volume simultaneously, either read-write or read-only. Volumes are only removed when you explicitly remove them.\
> \
> \- When you want to store your container’s data on a remote host or a cloud provider, rather than locally.\
> \
> \- When the Docker host is not guaranteed to have a given directory or file structure. Volumes help you decouple the configuration of the Docker host from the container runtime.

When we write the following in a `Dockerfile,` we tell docker that the content in this directory should outlive the container itself, until we manually remove the volume. The outgoing directory and storing logic will be handled by docker

```docker
VOLUME /var/lib/mysql
```

or, while running a container `docker run -v /var/lib/mysql <image name>` .

The prior example was from the official mysql image's Dockerfile. When we run a container from this image and inspect the container

```bash
docker container inspect <container id/name>
```

Under the `Mounts` array, we see all the volumes with their name, which is a unique string assigned by Docker, unique to the Docker host. We can also see the same volumes when we list all the available volumes under our docker host using. These are unnamed volumes.

```bash
docker volume ls
```

But, we don't have any way to know which volume goes to which container. Knowing this will be beneficial when we want to mount a volume to multiple container, for maybe, scalability reasons. This is where named volumes come into play.





**Bind Mounts**

> When you use a bind mount, a file or directory on the _host machine_ is mounted into a container. The file or directory is referenced by its full path on the host machine. The file or directory does not need to exist on the Docker host already. It is created on demand if it does not yet exist. Bind mounts are very performant, but they rely on the host machine’s filesystem having a specific directory structure available. If you are developing new Docker applications, consider using named volumes instead. You can’t use Docker CLI commands to directly manage bind mounts.
>
> One side effect of using bind mounts, for better or for worse, is that you can change the **host** filesystem via processes running in a **container**, including creating, modifying, or deleting important system files or directories. This is a powerful ability which can have security implications, including impacting non-Docker processes on the host system.\
> \
> Good use cases for bind mounts : \
> \- Sharing configuration files from the host machine to containers. This is how Docker provides DNS resolution to containers by default, by mounting `/etc/resolv.conf` from the host machine into each container.\
> \
> \- When the file or directory structure of the Docker host is guaranteed to be consistent with the bind mounts the containers require.

Unlike volumes, bind mounts can't be specified in the `Dockerfile,` it must be run with `docker container run.`&#x20;

```bash
docker container run -v /path/to/local:/path/to/container <image>
```

#### Overwrite dilemma with bind mount

Let's say we have some code in our `/app` folder inside of our container. When we mount a folder from the host to this `/app` folder, Docker doesn't actually start overwriting the folder, rather the local folder is mounted in the given directory, and all the previous stuff are concealed while the container is running.

\*\* node\_module scenario







