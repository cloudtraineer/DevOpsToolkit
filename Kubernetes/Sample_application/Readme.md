## Application Deployment using Minikube
The project aims to deploy a web application along with a MongoDB database in a Kubernetes environment using Minikube. It involves setting up MongoDB, creating Kubernetes resources (such as ConfigMaps, Secrets, Deployments, and Services), configuring access, and enabling interaction with the deployed application.
## Project Steps
1. Configuration and Secrets Setup
     a. Creation of a ConfigMap (mongodb-config) to store MongoDB configuration data, allowing easy modification and separation of configuration from Pod specifications.
     b. Generating a Secret (mongodb-secret) to securely store sensitive information like usernames and passwords for MongoDB
2. Deployment of MongoDB
3. Deployment of Web Application
4. Integration of Secrets and Configurations
5. External Access Configuration
6. Minikube Deployment:
7. Accessing the Web Application

   
## Project Diagram 
![alt text](https://github.com/cloudtraineer/Installation_guide/blob/master/Kubernetes/Sample_application/project.png?raw=true)
