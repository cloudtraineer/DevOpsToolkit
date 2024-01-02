# Install Kubernetes on AWS EC2
## Kubernetes Components
### Minikube
Minikube is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes.

### Kubectl
The Kubernetes command-line tool, kubectl, allows you to run commands against Kubernetes clusters. You can use kubectl to deploy applications, inspect and manage cluster resources, and view logs.

### Prerequisites
1. EC2 Instance
     - T2.medium and above
     - 30 GB EBS volume
2. Docker installed and running
3. Create a user with admin rights
4. Add the newly created user to the docker group
   ```sh
   usermod -aG docker <user_name>
   ```

## Install Minikube on EC2
1. Installation
```sh
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
2. Start your cluster
 ```sh
minikube start --driver docker
```
3. Check the Status of the cluster
 ```sh
minikube status
```
### Note: Kubectl gets installed as a dependency of minikube, if not installed follow the below porcess

## Install Kubectl on EC2
1. Download the latest release with the command
```sh
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
2. Install kubectl
```sh
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```
3. Test to ensure the version you installed is up-to-date:
```sh
kubectl version --client
```
