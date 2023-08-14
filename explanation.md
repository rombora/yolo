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
<br/>
<br/>
<br/>

# Configuration Management with Ansible and Vagrant
Here, an ansible playbook is used to automate the setup and deployment process of the applciation on a vagrant Virtual Machine.

## i. Vagrantfile
The vagrant file specifies the base image, geerlingguy/ubuntu2004, used to create the VM. It sets up port forwarding for the client and backend services at port 3000 and 5000 respectively. This allows access to the application on the host machine.

## ii. ansible.cfg
This is the configuration file for ansible. It defines the inventory file, path to the private key file, the remote user and host key checking value.

## iii. hosts
It defines the inventory of hosts for ansible. It holds the IP address and port to access the vagrant VM.

## iv. playbook.yml
This is the main ansible playbook. It consists of tasks that should be carried out to get the application to run. It makes use of **roles** to complete these tasks. The tasks include:
- Setup dependencies
  - Update apt cache
  - Install dependencies
- Clone app repository
- Run app

## v. vars.yml
It contains the variables used in the playbook and roles. The variables are:
- **app_repo**: the URL to the github repository with the application to setup and run
- **app_version**: the branch to be used from the application repository
- **app_dir**: the directory where the application is to be clone and setup

## vi. roles
This is a directory containing the roles that are performed by tasks in the playbook. The roles are:

- **update-cache**: Updates the system packages in the VM.
- **install-dependencies**: Install the necessary packages required to setup and run the application in the VM. These dependencies are docker and docker-compose.
- **clone-repository**: Clones the repository from github, into the VM using git.
- **run-app**: Starts running the application after everything has been setup in the VM. It executes the command `docker-compose up -d`.
<br/>
<br/>
<br/>

# Kubernetes Deployment and Orchestration
## Kubernetes objects
- **Deployment**: Used to manage deployment of both client and backend.
- **StatefulSets**: Manage the MongoDB. They are good for managing stateful applications and persist data.

## Backend Deployment and Service
To identify and manage the backend pod, it is labelled backend

The deployment uses a container that runs an image from dockerhub: **alvinrombora/yolo-backend:v1.0.1**

The backend container inside the pod listens on port **5000** to handle incoming requests.

For communication with the database, the environment variable **MONGODB_URI** is assigned the value **mongodb://mongo:27017/yolomy** to reference the mongo service

The backend kubernetes service is used to enable external access.

It exposes port **5000** for access by external clients.

## Client Deployment and Service
To identify the client pod, it is labelled client.

The client deployment uses the container **alvinrombora/yolo-client:v1.0.1**

The client is run on port **3000**.

It exposes port **3000** for external users to access the application.

## Mongo StatefulSet and Service
The application's MongoDB database is managed using StatefulSet which helps in persisting the application's data.

The StatefulSet is given a container **"mongo"** which uses the mongo docker image

The mongo container listens on port **27017** for database connections

Data is persisted on the volume mounted at **/data/db**

Port **27017** is exposed to allow for database connections





