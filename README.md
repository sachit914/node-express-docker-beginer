# node-express-docker-beginer

## cheat sheet

###  build image
```
docker build -t node-app-image .
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
list all stopped or running container
```
docker ps -a
```

### deleteing container
```
docker rm conatainerName -f
```


### execute docker container
```
docker exec -it containerName bash
```

```
exit
```

### Volumes

```
docker run -v pathtofolderonlocation:pathtofolderoncontainer -p 3000:3000 -d --name node-app node-app-image
```

### logs
```
docker logs containername
```
### list all volumes
```
docker volume ls
```

### delete unecessary volumes
```
docker volume prune
```

or delete volume when we delete container

```
docker rm node-app -fv
```
### create docker image by docker-compose
automantically runs 
```
docker-compose up -d --build
```
### delete docker container by docker-compose
```
 docker-compose down -v
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

```
const express = require("express")

const app = express()

app.get("/",(req,res)=>{
    res.send("<h2>Hello there</h2>")
})

const port= process.env.PORT|| 3000;

app.listen(port,()=> console.log(`listening on port ${port}`));

```


http://localhost:3000/

## creating custom docker image

building on top of node image

#### Steps

create file named Dockerfile

The WORKDIR instruction in a Dockerfile sets the current working directory for subsequent instructions in the Dockerfile. When the WORKDIR instruction is executed, all the subsequent instructions in the Dockerfile will be executed relative to the specified directory

```
FROM node:18
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
docker build -t node-app-image .
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
```
exit
```



## Docker ignore file

before remove container and rebuild image with.dockerignore file
after that eun container

```
node_modules
Dockerfile
.dockerignore
.git
.gitignore
```
```
docker build -t node-app-image .
```

```
docker exec -it node-app bash
```

## when some changes done on code              32: 00

approach 1

when some changes done on code we have to rebuild image

approach 2

second approach is to use volume

bind mount volume: sync locaol folder from local machine to folder system on docker system

```
docker rm node-app -f 
node-app
```

```
docker run -v pathtofolderonlocation:pathtofolderoncontainer -p 3000:3000 -d --name node-app node-app-image
```
```
docker run -v D:\docker\docker nodejs express\:/app -p 3000:3000 -d --name node-app node-app-image
```
```
docker run -v ${pwd}:/app -p 3000:3000 -d --name node-app node-app-image
```
#### use this
```
docker run -v ${pwd}:/app:ro -v/app/node_modules -p 3000:3000 -d --name node-app node-app-image
```

```
docker exec -it node-app bash
```

```
cat index.js
```
## dev dependency nodemon
```
npm install nodemon --save-dev
```
```
  "scripts": {
    "start":"node index.js",
    "dev":"nodemon -L index.js"
  },
```
```
FROM node:18
WORKDIR /app
COPY package.json .
RUN npm install
#copy all files from current folder
COPY . ./            
EXPOSE 3000
CMD ["npm","run","dev"]
```

```
docker logs node-app
```

## environment variables in docker   55: 19

```
FROM node:18
WORKDIR /app
COPY package.json .
RUN npm install
#copy all files from current folder
COPY . ./            
ENV PORT 3000
EXPOSE $PORT
CMD ["npm","run","dev"]
```

```
docker run -v ${pwd}:/app:ro -v/app/node_modules --env PORT=4000 -p 3000:4000 -d
 --name node-app node-app-image
```

```
docker exec -it node-app bash
```

```
printenv
```

other approach is create a file that stores all our environment variable

create .env file

```
docker run -v ${pwd}:/app:ro -v/app/node_modules --env-file ./.env -p 3000:4000 -d --name node-app node-app-image
```


## docker compose file          1: 06

create docker-compose.yml file

replacment for docker run -v ${pwd}:/app:ro -v/app/node_modules --env-file ./.env -p 3000:4000 -d --name node-app node-app-image

```
version:"3"
# version of docker compose
# specififying all the containers we want to create
services:
#node-app is name of container
  node-app:
    #what image are we using
    build: .         #in current folder dockerfile
    ports:
      - "3000:3000"
    volumes:
      - ./:/app
      - /app/node_modules
    environment:
      - PORT=3000
```
```
docker-compose up -d --build
```

```
 docker-compose down -v
```

## production release



create docker-compose.dev.yml
docker-compose.prod.yml                                                                  prod stands for production
docker-compose.yml files


### in docker-compose.yml
```
version: "3"
services:
  node-app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - PORT=3000
```
### in docker-compose.dev.yml
```
version: "3"
services:
  node-app:
    volumes:
      - ./:/app
      - /app/node_modules
    environment:
      - NODE_ENV=developement
    command: npm run dev
```

### in docker-composoe.prod.yml
```
version: "3"
services:
  node-app:
    environment:
      - NODE_ENV=production
    command: node index.js
```



### in dockerfile
```
FROM node:18
WORKDIR /app
COPY package.json .
RUN npm install
#copy all files from current folder
COPY . ./            
ENV PORT 3000
EXPOSE $PORT
CMD ["node","index.js"]
```

### to run  devlopment ready code
```
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up -d
```
```
 docker-compose -f docker-compose.yml -f docker-compose.dev.yml down -v
```

### to run  production ready code

```
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```
since in production there is no volume attached 

so need to rebuild image
```
 docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d --build
```

1:34:33  min



