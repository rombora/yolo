# Base images
## i. Client container
  Client uses node 13.12.0-alpine image. This is because the client package.json states the required node version to use with the application, node 13.12.0.
  The reason behind chosing alpine is that it provides a lightweight node environment which helps make the image smaller and faster to start a container.
## ii. Backend container
  Backend also uses node 13.12.0-alpine, not because it is a requirement for the backend application, but for uniformity.
## iii. Mongo container
  Mongo container uses the mongo image to easily setup MongoDB for the application without any further installations and configurations.

# Dockerfile directives
## i. Client Dockerfile
- **FROM node:13.12.0-alpine**: Sets the base image to be used.
- **WORKDIR /app/client**: Sets the working directory to /app/client.
- **COPY package.json package.json**: Copies the package.json file to the working directory.
- **RUN npm install**: Installs the necessary dependencies using the package.json file.
- **COPY . .**: Copies all the files from the current directory to the working directory in the container.
- **EXPOSE 3000**: Exposes port 3000 to access the app.
- **CMD ["npm", "start"]**: Runs npm start when the container starts.

## ii. Backend Dockerfile
- **FROM node:13.12.0-alpine**: Sets the base image to be used.
- **WORKDIR /app/backend**: Sets the working directory to /app/backend.
- **COPY package.json package.json**: Copies the package.json file to the working directory.
- **RUN npm install**: Installs the necessary dependencies using the package.json file.
- **COPY . .**: Copies all the files from the current directory to the working directory in the container.
- **EXPOSE 5000**: Exposes port 5000 to access the app.
- **CMD ["npm", "start"]**: Runs npm start when the container starts.

# Network
## yolo-network
  A bridge network is used to bridge communication between the three containers, the client, backend and mongo.

# Volume
## mongo-volume
  A local volume is used to persist MongoDB data.

# Naming Standards
  To ensure ease of identification of Docker images and containers, the semver versioning system is used for the image tags.
  Here, versioning starts from v1.0.0.
