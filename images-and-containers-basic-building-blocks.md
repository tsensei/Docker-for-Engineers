# Images & Containers : Basic Building Blocks

A **Docker image** is an immutable file that contains source code, libraries, dependencies, tools and other files needed for an application to run.

**Images** are just _read-only_ templates for building **containers.**

When user runs an image, it becomes one or multiple container instances.

An image is really a template that can be turned into a container. To turn an image into a container, the Docker engine takes the image, adds a read-write filesystem on top and initialises various settings including network ports, container name, ID and resource limits. A running container has a currently executing process, but a container can also be stopped (or _exited_ in Docker's terminology). An exited container is _not_ the same as an image, as it can be restarted and will retain its settings and any filesystem changes.

### Building a Docker Image

We can create a docker image based on a base image from docker hub including our application code and all the necessary dependencies.

Example of a Express app listening on port 80 when dockerized

```docker
FROM node
# base image

WORKDIR /app
# setting a working directory
# all the following command will be executed relative to this directory

COPY . .
# as we write this dockerfile inside of our code folder
# all of the application code is copied to the workdir

RUN npm install
# install all necessary dependencies
# RUN is used to run commands while building a image

EXPOSE 80
# defines which port(s) will be exposed
# mainly for documentation purpose

CMD ["node", "server.js"]
# default command when the container is started
# CMD always comes last in a dockerfile

```

### Understanding Image layers

