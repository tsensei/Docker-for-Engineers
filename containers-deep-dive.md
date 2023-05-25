# Containers : Deep Dive

## Container Lifecycle

```bash
docker container create
# creates a container from a image

docker container start
# starts a stopped container
# starts in detached mode in default as opposed to docker run
# use '-a -i' to attach

docker container run
# create + start
# starts in attach mode with stdout, stderr attached
# '-it' flag to get a interactive terminal

docker container attach
# attach stdin, stdout & stderr to a running container

docker container stop
# exits the container gracefully 
# giving it time to do some cleanup using the SIGTERM command

docker container kill
# exits immediately with SIGKILL

## docker stop gives the container 10 seconds to exit, 
## if it doesn't, it is issued with a docker kill

docker container ls
# lists all running containers
# using '-a' lists all running and stopped containers

docker container remove
# removes stopped container
# using '-f' stops running container too

docker container prune
# removes all stopped containers
```

### Breakdown of a command

```bash
docker container run --publish 80:80 --detach --name webhost nginx

# run -> create + start
# publish hostPort:containerPort -> binds a container port to host port
# detach -> runs in detached mode, doesn't hang the command line
# name -> provides a name to the container to reference in future
```

#### Removing containers

```bash
docker container rm <name/id>

# don't need to give total id, can give only 2/3 digits as long as they are unique
```

#### Container Monitoring

```bash
docker container logs <name/id>
# return logs of a detached container

docker container top
# returns process list of container

docker container inspect
# returns details of container config

docker container stats
# returns performance stats for all containers
```

#### Getting shell inside containers

```bash
docker container run -it <image> <command>
# starts a new container interactively
# if command not specified, it runs the default command

docker container exec -it <name/id> <command>
# runs additional commands in running container
```

#### Naming container

```bash
docker run --name <name> <image>
```
