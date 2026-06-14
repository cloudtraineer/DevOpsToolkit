# Create an Amazon EKS Cluster from AWS Console

## Overview

This guide explains how to create a basic Amazon EKS cluster using the AWS Management Console.

### Assumptions

- An existing VPC already exists.
- Public and private subnets are already configured.
- Internet Gateway and NAT Gateway are already configured.
- No AWS Load Balancer will be used.
- A basic EKS cluster with a Managed Node Group will be created.
---

# Prerequisites

## Existing VPC Requirements

Verify the VPC has:

- Minimum 2 subnets in different Availability Zones
- DNS Resolution Enabled
- DNS Hostnames Enabled
- Proper Route Tables
- Internet Connectivity

Navigate to:

```text
VPC → Your VPC → Actions → Edit VPC Settings
```
Ensure:

- DNS Resolution = Enabled
- DNS Hostnames = Enabled

---

# Step 1: Create EKS Cluster IAM Role

Navigate to:

```text
IAM → Roles → Create Role
```

### Trusted Entity

```text
AWS Service
```

### Use Case

```text
EKS
```

### Permissions

Attach:

```text
AmazonEKSClusterPolicy
```

### Role Name

```text
EKSClusterRole
```

Create the role.

---

# Step 2: Create EKS Node Group IAM Role

Navigate to:

```text
IAM → Roles → Create Role
```

### Trusted Entity

```text
AWS Service
```

### Use Case

```text
EC2
```

### Attach Policies

```text
AmazonEKSWorkerNodePolicy
AmazonEC2ContainerRegistryReadOnly
AmazonEKS_CNI_Policy
```

### Role Name

```text
EKSNodeGroupRole
```

Create the role.

---

# Step 3: Create EKS Cluster

Navigate to:

```text
AWS Console → EKS → Clusters
```

Click:

```text
Create Cluster
```

---

## Cluster Configuration

### Cluster Name

Example:

```text
mycluster
```

### Kubernetes Version

Select the latest supported version.

### Cluster IAM Role

Select:

```text
EKSClusterRole
```

Click:

```text
Next
```

---

# Step 4: Configure Networking

### VPC

Select your existing VPC.

### Subnets

Select at least two subnets across different Availability Zones.

Example:

```text
Private Subnet A
Private Subnet B
```

### Security Group

Create or select:

```text
eks-cluster-sg
```

### Cluster Endpoint Access

Recommended:

```text
Public and Private
```

For production:

```text
Private Only
```

Click:

```text
Next
```

---

# Step 5: Configure Observability (Optional)

Enable:

- API
- Audit
- Authenticator
- Scheduler
- Controller Manager

These logs are stored in CloudWatch.

Click:

```text
Next
```

---

# Step 6: Review and Create Cluster

Review all settings.

Click:

```text
Create
```

Wait approximately 10-15 minutes.

Cluster status should become:

```text
Active
```

---

# Step 7: Create Managed Node Group

Navigate to:

```text
EKS Cluster
→ Compute
→ Add Node Group
```

---

## Node Group Details

### Name

```text
default-node-group
```

### Node IAM Role

Select:

```text
EKSNodeGroupRole
```

Click:

```text
Next
```

---

## Compute Configuration

### Capacity Type

```text
On-Demand
```

### AMI Type

```text
Amazon Linux 2023
```

### Instance Type

Example:

```text
t3.medium
```

### Disk Size

```text
20 GB
```

Click:

```text
Next
```

---

## Scaling Configuration

Example:

```text
Desired Size: 1
Minimum Size: 1
Maximum Size: 2
```

Click:

```text
Next
```

---

## Networking

Select the same private subnets used by the cluster.

Click:

```text
Create
```

Wait until Node Group status becomes:

```text
Active
```
# Summary

You have successfully:

- Created an Amazon EKS Cluster using an existing VPC
- Created required IAM Roles
- Configured networking
- Created a Managed Node Group
