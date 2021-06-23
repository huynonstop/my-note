# .dockerignore

```bash
node_modules # or
*/node_modules
```

# Dockerfile

instructions to tell docker how to build a docker image

every instruction is a step or layer and it will be cached by docker

## FROM baseImage

```dockerfile
FROM baseImage
FROM unbuntu
FROM node
```

## WORKDIR

default root folder in container file system

```dockerfile
WORKDIR /absolute/path/to/workdir
WORKDIR relative/path

WORKDIR /app
```

## COPY

copy files or folder from host to container file system (exclude file or folder in .dockerignore)

```dockerfile
COPY . /absolute/path # All in host to absolute path, recommended
COPY . relative/to/workdir # All in host to path from workdir

COPY . /app # All to folder app in the image

COPY . ./ # All in host to current WORKDIR
COPY package.json ./ # package.json in host to current WORKDIR
```

## RUN

execute command inside container shell at WORKDIR when created container 

```dockerfile
RUN npm install
```

## CMD 

execute command only when started a container, not when created

default execute CMD of the base image

this will not be cached when build images

```dockerfile
CMD ["node","index.js"]
CMD ["npm","start"]
```

## ENV

`ENV port=8080`

## EXPOSE

expose port this container will listen on at runtime

```dockerfile
EXPOSE 8080
EXPOSE 3000 8000
EXPOSE 9000-9005
```

## Example

```.dockerignore
# .dockerignore
node_modules
```

```dockerfile
FROM node:12

WORKDIR /app

COPY package.json /app # cached dependency
RUN npm install # cached npm install

COPY . /app

ENV port=8080
EXPOSE 8080
CMD ["npm", "start"] 
```

# 