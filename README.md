## demo app - developing with Docker

This demo app shows a simple user profile app set up using

- index.html with pure js and css styles
- nodejs backend with express module
- mongodb for data storage

All components are docker-based

### With Docker

#### To start the application

Step 1: Create docker network

```bash
    docker network create mongo-network
```

Step 2: start mongodb

```bash
    docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo
```

Step 3: start mongo-express

```bash
    docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongodb mongo-express
```

_NOTE: creating docker-network is optional. You can start both containers in a default network. In this case, just emit `--net` flag in `docker run` command_

Step 4: open mongo-express from browser

```js
    http://localhost:8081
```

Step 5: create `user-account` _db_ and `users` _collection_ in mongo-express

Step 6: Start your nodejs application locally - go to the `app` directory of project

```bash
    npm install
    node server.js
```

when using `nodemon` as a _dev_ dependency use

```bash
    npm run dev
```

Step 7: Access you nodejs application UI from browser

```js
    http://localhost:3000
```

### With Docker Compose

#### To start the application

Step 1: start mongodb and mongo-express

```bash
    docker-compose -f docker-compose.yaml up
```

_Note: that the `-f` flag points to the file that contains the docker compose commands_

## using docker-compose to stop all connected containers at once

```bash
docker-compose -f docker-compose.yaml down
```

_You can access the mongo-express under localhost:8080 from your browser_

Step 2: in mongo-express UI - create a new database `my-db`

Step 3: in mongo-express UI - create a new collection `users` in the database `my-db`

Step 4: start node server

```bash
    npm install
    node server.js
```

Step 5: access the nodejs application from browser

```js
    http://localhost:3000
```

#### To build a docker image from the application

```bash
    docker build -t my-app:1.0 .
```

The dot `.` at the end of the command denotes location of the Dockerfile.

_Note: that when using `docker-compose` command, the `default network` will be created every time you start a new container and stop every time you stop a container_

For testing always check the network before and after running a container to make sure

```bash
    docker network ls
```

_Note: data in a container is not persistence except when using `docker volumes`_
