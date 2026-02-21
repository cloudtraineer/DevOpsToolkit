# Connect to Amazon EKS Cluster from an EC2 Instance (IAM Role Based Access Only)

This guide explains how to connect to an Amazon EKS cluster using an EC2
machine **using IAM Role-based authentication only (no AWS access
keys).**

------------------------------------------------------------------------

## Architecture Overview

EC2 (IAM Role Attached) → AWS CLI → EKS API Server → Worker Nodes

------------------------------------------------------------------------

## Prerequisites

-   An existing EKS Cluster
-   An EC2 instance (Amazon Linux 2 / 2023 recommended)
-   IAM Role attached to EC2 with the following permissions:

### Required IAM Policies

Attach these policies to the EC2 IAM Role:

-   AmazonEKSClusterPolicy
-   AmazonEKSWorkerNodePolicy (if needed)
-   AmazonEC2ContainerRegistryReadOnly

For admin-level kubectl access, ensure the IAM Role is mapped in the
`aws-auth` ConfigMap.

⚠️ No AWS Access Keys should be configured on the EC2 instance.

------------------------------------------------------------------------

## Step 1: Attach IAM Role to EC2

1.  Go to AWS Console → EC2 → Instances
2.  Select your EC2 instance
3.  Click **Actions → Security → Modify IAM Role**
4.  Attach the required IAM Role
5.  Save changes

------------------------------------------------------------------------

## Step 2: SSH into EC2 Instance

``` bash
ssh -i your-key.pem ec2-user@<EC2-Public-IP>
```

------------------------------------------------------------------------

## Step 3: Install Required Tools

### Update System

``` bash
sudo yum update -y
```

### Install AWS CLI (If Not Installed)

``` bash
sudo yum install -y aws-cli
aws --version
```

Verify IAM Role is working:

``` bash
aws sts get-caller-identity
```

You should see the IAM Role ARN in the output.

------------------------------------------------------------------------

### Install kubectl

``` bash
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/latest/2023-11-14/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client
```

------------------------------------------------------------------------

### Install eksctl (Optional)

``` bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```

------------------------------------------------------------------------

## Step 4: Update kubeconfig Using IAM Role

Since EC2 has an IAM Role attached, no manual AWS configuration is
required.

Run:

``` bash
aws eks update-kubeconfig --region <region> --name <cluster-name>
```

Example:

``` bash
aws eks update-kubeconfig --region ap-south-1 --name my-eks-cluster
```

This command: - Retrieves cluster endpoint - Updates \~/.kube/config -
Configures kubectl authentication using IAM Role

------------------------------------------------------------------------

## Step 5: Verify Connection

``` bash
kubectl get nodes
```

If configured correctly, worker nodes will appear in **Ready** state.

------------------------------------------------------------------------

## Step 6: Map IAM Role in aws-auth (If Unauthorized Error)

If you get:

"You must be logged in to the server (Unauthorized)"

Then your IAM Role is not mapped in EKS.

Check:

``` bash
kubectl get configmap aws-auth -n kube-system -o yaml
```

To edit:

``` bash
kubectl edit configmap aws-auth -n kube-system
```

Add your IAM Role under `mapRoles`.

Example:

``` yaml
- rolearn: arn:aws:iam::<account-id>:role/<EC2-Role-Name>
  username: ec2-admin
  groups:
    - system:masters
```

