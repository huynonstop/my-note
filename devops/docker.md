https://www.youtube.com/watch?v=gAkwW2tuIqE

https://www.youtube.com/watch?v=1xo-0gCVhTU

[Docker and Kubernetes - Full Course for Beginners - YouTube](https://www.youtube.com/watch?v=Wf2eSG3owoA)

[Docker For Everyone In 2 Hours\] - YouTube](https://www.youtube.com/watch?v=d-PPOS-VsC8)

[TechWorld with Nana - YouTube](https://www.youtube.com/c/TechWorldwithNana/videos)

# VM

![image-20210322172628984](assets/docker/image-20210322172628984.png)

![image-20200825130341150](assets/docker/image-20200825130341150.png)

![image-20210322172722992](assets/docker/image-20210322172722992.png)

# Docker

![image-20210322165951376](assets/docker/image-20210322165951376.png)

![image-20210322170931077](assets/docker/image-20210322170931077.png)

reproducing environment 

package software into containers that run reliably in any environment

![image-20210322180215914](assets/docker/image-20210322180215914.png)

![image-20200825130509290](assets/docker/image-20200825130509290.png)

![image-20210322180332132](assets/docker/image-20210322180332132.png)

# Images

template (snapshot) of your app to run in container

# Container

running process

=> 1 process 1 container

# Volumes

data and files created in container will lost if stop container

=> Volumes (shared data across container), a folder on host machines and other containers mount to CRUD theses files

![image-20200825124349770](assets/docker/image-20200825124349770.png)

# Port Forwarding

allow host machine access container

docker commands

docker compose

# DockerFile

blueprint to tell docker how to build a docker image

### layer

every instruction is a step or layer

docker will cache layer

### .dockerignore

`node_modules`

## FROM

`FROM unbuntu`

`FROM node`

## WORKDIR

`WORKDIR /app`

## COPY

`COPY (local place) (container place/ working directory)`

`COPY package*.json ./`

`COPY . .`

## RUN

`run npm install`

## ENV

`ENV port=8080`

## EXPOSE

`EXPOSE 8080`

## CMD 

run command

`CMD ["npm","start"]` 

## Example

.dockerignore

```
node_modules
```

DockerFile

```dockerfile
FROM node:12

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

ENV port=8080
EXPOSE 8080
CMD ["npm", "start"] 
```



# Pull

# Push

# Commands

## `docker ps`

## `docker build (-flags) DOCKERFILE_PATH`

-t name: name tag (username/app_name:version)

`docker build huy/abc:1.0 ./`

## `docker volume create NAME`

## `docker run (-flags) DOCKERFILE_PATH`

-p **local_port : container_port** :  **port forwading**

--mount source=**VOLUME_NAME**,target=**FILE_IN_CONTAINER** : mount to volumes 

# Docker Compose

run multiple Docker container at a same time

`docker-compose.yml`

```yml
version: '3'
services: (each key is a container)
	web:
		build: . (path to dockerfile of this container)
		ports: (port Forwarding)
			- "8080:8080"
	db:
		image: "mysql"
		environment:
			MYSQL_ROOT_PASSWORD: password
		volumes: (mount volumes to db container)
			- db-data:/foo
			
volumes:
	db-data:
```

