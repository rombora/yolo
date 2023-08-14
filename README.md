# Requirements
Make sure that you have the following installed:
- [node](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-18-04) 
- npm 
- [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) and start the mongodb service with `sudo service mongod start`

## Navigate to the Client Folder 
 `cd client`

## Run the folllowing command to install the dependencies 
 `npm install`

## Run the folllowing to start the app
 `npm start`

## Open a new terminal and run the same commands in the backend folder
 `cd ../backend`

 `npm install`

 `npm start`

 # Running a containerized version of the application
 From the root directory of the application, run
 
 `sudo docker compose up`
 
 This will pull the necessary images from dockerhub and run the application.


 # Running the application with Ansible playbook in a Vagrant Virtual Machine
 ## Prerequisites
 - Vagrant
 - VirtualBox

 ## Start VM
 Run the following command to get everything setup and started in the VM.
 
 `vagrant up`

 This will create a virtual machine, and get everything setup. When it's done, the application should now be up and accessed in the web browser:
 
 **http://localhost:3000**
 <br/>
 

# Running the application in Kubernetes container
The application is running on http://41.212.75.76:3000

## Create a cluster
`gcloud container cluster create-auto yolo-cluster --region us-central1`

## Apply the configurations
Navigate to `/manifests` dir
``cd manifests```
```
kubectl apply -f mongo_statefulset.yml
kubectl apply -f mongo_service.yml
kubectl apply -f backend_deployment.yml
kubectl apply -f backend_service.yml
kubectl apply -f client_deployment.yml
kubectl apply -f client_service.yml
```

After that, we can now access the application from the provided url

In our case, we can access it [here](http://41.212.75.76:3000)
<br/>
 
### Go ahead a nd add a product (note that the price field only takes a numeric input)
