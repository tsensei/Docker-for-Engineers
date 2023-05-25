# Networks





#### Logging exposed port from a container

```bash
docker container run -p 80:80 --name webhost -d nginx

docker container port webhost
```

#### Logging IP address of a container

```bash
docker container inspect --format '{{ .NetworkSettings.IPAddress}}' webhost
```

#### Docker Network commands

```bash
docker network ls
# show networks

docker network inspect
# inspect a network

docker network create --driver
# create a network

docker network connect
# attach a network to a container

docker network disconnect
# detach a network from container
```

