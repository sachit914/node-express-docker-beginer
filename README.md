# node-express-docker-beginer


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
docker build .
```

## cheat sheet

### list all images
```
docker image ls
```

### remove image
```
docker image rm
```
