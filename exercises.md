# Docker exercises
These exercises support the introduction to docker during the jx-workshop

## 1. Use docker to support your local development
During development you might frequently run the application on your development machine (laptop), however there is the dependency on a database and we
don't want to install all kinds of databases on our host (even different projects might require different DB configurations)

### Issue demonstration
The snack-service is such an application. Let's try to run it to demonstrate the issue [run snacks service local](https://github.com/Marthijs-Berfelo/jx-ws-snacks#1-on-local-host-gradlebuild)

### Solution: Run db container
```bash
docker run -v ~/jx-workshop/Docker-Volumes/mongo:/data/db -p 27017:27017 mongo:4.0.8
```

### Solution demonstration
Restart the running application `CTRL+c` and start the application as you did during the issue demonstration

In your browser go to: `http://localhost:8080/graphiql

## 2. How to run docker containers like a service
When the docker service process is restarted (i.e. laptop restart) the db container is not restarted. If you forget to recreate the container you might endup debugging the issue unnecessarily.

### Issue demonstration
Stop and restart your docker service (stop and start the VM or the docker daemon process) and stop and restart the application.

### Solution: Run db container using docker swarm stack
1. Initiate docker swarm
   ```bash
   docker swarm init
   ```
1. Deploy db stack
   ```bash
   docker stack deploy -c ~/jx-workshop/Development/jx-ws-dockermongo-stack.yaml mongo-dev
   ```

### Solution demonstration
Restart the running application `CTRL+c` and start the application as you did during the issue demonstration

In your browser go to: `http://localhost:8080/graphiql

## 3. Run an application in docker
When the application is going to run in a container runtime in production, you will probably want to check the container locally before committing Dockerfile changes.

### Issue demonstration
In the root of the snacks service project build and deploy the snacks container by running:
``` bash
docker build . -t local/snacks:latest
docker run -p 8080:8080 local/snacks:latest
```

In the output of the run command you will see a similar error as you did during the issue demonstration of exercise 1.

### Solution: Run application using docker swarm stack with the db as additional service
1. Remove the mongo-dev stack
   ```bash
   docker stack rm mongo-dev
   ```
1. Create the application stack
   [run application stack](https://github.com/Marthijs-Berfelo/jx-ws-snacks#2-in-local-docker-container)

### Solution demonstration
In your browser go to: `http://localhost:9000/graphiql`

## END: Clean up
Once your done exploring docker [docker commandline reference](https://docs.docker.com/engine/reference/commandline/docker/), remove
the application stack
```bash
docker stack rm snacks-dev
```
