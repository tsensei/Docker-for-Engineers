# Dev Setup

## Production grade CI/CD workflow with Docker and Github

#### We will have a Github Repo with two branches :

* feature : Development branch - make changes to code
* master : Deployment branch

### Adding a new feature :

1. Git Pull the feature branch
2. Add some code
3. Git Push to the feature branch
4. Create pull request to master branch
   1. Test with Travis CI
   2. Push it over to hosting

### Development Setup

At this step, we will have a Dockerfile with the development config named `Dockerfile.dev`, whose contents are :

```Dockerfile
FROM node:alpine
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
CMD [ "npm", "start" ]
```

Now, if we want to build an image out of this dockerfile using `docker build .`, we will get an error as the build command will look for a `Dockerfile` with no extensions. To specify our dev Dockerfile, we use :\
`docker build Dockerfile.dev .`

With all the volume and port configuration, our `docker run` looks like :\
`docker run -p 3000:3000 -v /app/node_modules -v $(pwd):/app <image_id>`

We can simplify the run command with docker compose.

The two long `docker build` and `docker run` command can be simplified with a `docker-compose.yml` :

```
version: "3"
services:
  web:
    build:
      context: .
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
    volumes:
      - /app/node_modules
      - .:/app
```

#### Running tests :

A prospective solution :

1. Start the project with `docker compose up`
2. List the running containers using `docker ps`
3. Get the id and execute command using `docker exec -it <container_id> npm run test`

But a better solution would be using another service in `docker-compose.yml` for running the test with a `command` argument set to `["npm","run","test"]`

### Production environment :

In a react environment, we would like to serve the build directory. To do that, we might use nginx.

So the problem that arises - we need to use node:alpine to run the `npm run build`, but also need nginx to serve the build directory ? Multi Step Build comes into play here.

We create a new dockerfile `Dockerfile` for production with multi stage build :

```Dockerfile
FROM node:alpine as builder
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
RUN npm run build

FROM nginx
COPY --from=builder /app/build /usr/share/nginx/html
```

Then we build the image using `docker build .` And serve using `docker run -p 8080:80 <image_id>`
