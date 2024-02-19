# Installation
## Build the Jenkins (Controller) Docker Image
```bash
docker build -t myjenkins-blueocean:2.414.2 .
```

## Create the network 'jenkins'
```bash
docker network create jenkins
```

## Run the Container
```bash
docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.414.2
```

## Get the Initial Admin Password
```bash
docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
```

## alpine/socat container to forward traffic from Jenkins to Docker Desktop on Host Machine
```bash
docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock
docker inspect <container_id> | grep IPAddress
```
This ip address will be used in the Jenkins Docker Plugin Configuration.

## Setup a docker container as build agent (on my Docker Host)
### Jenkins Docker Plugin Configuration when running jenkins as container

1. First Install Docker Plugin.

2. Go to Manage Jenkins -> System Configuration -> Scroll down to botton -> Add Cloud -> Docker.

3. If you are running jenkins as container, in the docker host uri field you have to enter unix or tcp address of the docker host. But since you are running jenkins as container, the container can't reach docker host unix port.

4. So, we have to run another container that can mediate between docker host and jenkins container. It will public docker host's unix port as its tcp port. Follow the instructions to create socat container https://hub.docker.com/r/alpine/socat/

5. After creating the socat container, you can go back the docker configuration in jenkins and enter tcp://socat-container-ip:2375

6. Test Connection should succeed now.
