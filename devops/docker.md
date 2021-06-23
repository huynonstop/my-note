https://www.youtube.com/watch?v=gAkwW2tuIqE

https://www.youtube.com/watch?v=1xo-0gCVhTU

[Docker and Kubernetes - Full Course for Beginners - YouTube](https://www.youtube.com/watch?v=Wf2eSG3owoA)

[Docker For Everyone In 2 Hours\] - YouTube](https://www.youtube.com/watch?v=d-PPOS-VsC8)

[TechWorld with Nana - YouTube](https://www.youtube.com/c/TechWorldwithNana/videos)

![image-20210322170931077](assets/docker/image-20210322170931077.png)

https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes

https://snyk.io/blog/10-best-practices-to-containerize-nodejs-web-applications-with-docker/

# Docker

![image-20210322165951376](assets/docker/image-20210322165951376.png)

reproducing environment , code

package software into containers that run reliably in any environment

![image-20210322180215914](assets/docker/image-20210322180215914.png)

![image-20200825130509290](assets/docker/image-20200825130509290.png)

![image-20210322180332132](assets/docker/image-20210322180332132.png)

## Images

get from docker-hub or create a custom image (write Dockerfile based on another images)

template (snapshot, setup, code, environment) of your app

read-only, need to rebuild for new update

layer base, cached same instruction in Dockerfile, when a layer changed rerun all layers after it. (more speed)

Images are "blueprints" for containers which then are running instances with read and write access.

## Container

the running instances base on images

containers are isolated from host environment, has an internal network and containers are separated from each other and have no shared data or state by default. (Isolation)

Images & Container concept allows multiple containers to be based on the same image without interfering with each other.

![image-20210618230252821](assets/docker/image-20210618230252821.png)

each docker container will run the source code and environment in the images

![image-20210623015628844](assets/docker/image-20210623015628844.png)

![image-20210618230203183](assets/docker/image-20210618230203183.png)

![image-20210623015027258](assets/docker/image-20210623015027258.png)

![image-20210623021326587](assets/docker/image-20210623021326587.png)

## Volumes

data and files created in container will lost if stop container

=> Volumes (shared data across container), a folder on host machines and other containers mount to CRUD theses files

![image-20200825124349770](assets/docker/image-20200825124349770.png)

## Port Mapping (Forwarding)

allow host machine access container port

docker commands

docker-compose

## Dockerfile

[dockerfile.md](./dockerfile.md)

## Commands

```bash	
# Images
docker pull image_base # pull image from docker-hub
docker build ./path/to/Dockerfile # build image base on dockerfile

docker images 
-a # all images

docker rmi image_id_or_name image_id_or_name
docker image prune

docker rmi $(docker images -a -q)

# Containers
docker ps 
-a # all container

docker rm container_id_or_name container_id_or_name

docker run 
-p host_port:container_expose_port # publish to host port
--rm # remove container after exist
image_id_or_name # Run container from image

docker stop container_id_or_name container_id_or_name

docker rm $(docker ps -a -f status=exited -q) # Remove all exited containers
docker stop $(docker ps -a -q) # Stop all containers
docker rm $(docker ps -a -q) # Remove all containers
```

For **all docker commands** where an **ID** can be used, you **don't always have to copy** / write out the **full id.**

You can also **just use the first (few) character(s)** - just enough to have a unique identifier.

So instead of

```
docker run abcdefg
```

you could also run

```
docker run abc
```

or, if there's no other image ID starting with "a", you could even run just:

```
docker run a
```

**This applies to ALL Docker commands where IDs are needed.**

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

