# node-express-docker-beginer4

## cheat sheet

###  build image
```
build -t node-app-image
```

### list all images
```
docker image ls
```

### remove image
```
docker image rm
```

### run image (if image is running then it is called container

```
docker run -p 3000:3000 -d --name node-app node-app-image  
```

3000:300 sending traffic comming from locolhost 3000 to docker port 3000

node-app is container name

### list of containers
```
docker ps
```

### deleteing container
```
docker rm node-app -f
```


### execute docker container
```
docker exec -it node-app bash
```
----------------------------------------------------------------------------------------->

### set up 
```
npm init y
```

```
npm i express
```

```
node index.js
```

http://localhost:3000/

## creating custom docker image

building on top of node image

#### Steps

create file named Dockerfile

The WORKDIR instruction in a Dockerfile sets the current working directory for subsequent instructions in the Dockerfile. When the WORKDIR instruction is executed, all the subsequent instructions in the Dockerfile will be executed relative to the specified directory

```
FROM node:latest
WORKDIR /app
COPY package.json .
RUN npm install
#copy all files from current folder
COPY . ./            
EXPOSE 3000
CMD ["node","index.js"]
```

#### creating image

```
build -t node-app-image
```

```
docker image ls
```

### container
```
docker run -p 3000:3000 -d --name node-app node-app-image  
```
```
docker ps
```

### execute docker container
```
docker exec -it node-app bash
```

```
ls
```

## Docker ignore file
