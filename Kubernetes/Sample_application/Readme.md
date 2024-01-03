## Application Deployment using Minikube
The project aims to deploy a web application along with a MongoDB database in a Kubernetes environment using Minikube. It involves setting up MongoDB, creating Kubernetes resources (such as ConfigMaps, Secrets, Deployments, and Services), configuring access, and enabling interaction with the deployed application.
## Project Steps
1. Configuration and Secrets Setup <br>
     a. Creation of a ConfigMap (mongo-config.yaml) to store MongoDB configuration data, allowing easy modification and separation of configuration from Pod specifications.<br>
     b. Generating a Secret (mongo-secret.yaml) to securely store sensitive information like usernames and passwords for MongoDB
2. Deployment of MongoDB <br>
     a. Definition of a MongoDB Deployment and Service (mongo.yaml) in Kubernetes to ensure the availability and accessibility of the MongoDB instance
3. Deployment of Web Application <br>
     a. Setup of a Deployment and Service (webapp.yaml) for the web application within Kubernetes.
4. Integration of Secrets and Configurations <br>
     a. Integration of the mongo-secret with the MongoDB Deployment, enabling the secure passing of sensitive credentials to the MongoDB instance.<br>
     b. Integration of the mongo-config ConfigMap in the WebApp Deployment, allowing the web application to access necessary configuration data.
5. External Access Configuration <br>
     a. Configuring external access to the deployed WebApp Service, potentially using NodePort or LoadBalancer configurations to enable access from outside the Kubernetes cluster.
6. Minikube Deployment <br>
     a. Execution of the deployment within Minikube by applying the Kubernetes resources to the local cluster using kubectl apply.
   ```sh
   minikube start
   kubectl apply -f mongo-config.yaml
   kubectl apply -f mongo-secret.yaml
   kubectl apply -f mongo.yaml
   kubectl apply -f webapp.yaml
   ```
7. Accessing the Web Application <br>
    a. Providing instructions on how to access the deployed web application in a browser by utilizing port-forwarding or the externally exposed endpoint.
    ```sh
    kubectl port-forward --address 0.0.0.0 service/<webapp_service_name> <browser_port>:<service_port> &
    ```

   
## Project Diagram 
![alt text](https://github.com/cloudtraineer/Installation_guide/blob/master/Kubernetes/Sample_application/project.png?raw=true)
