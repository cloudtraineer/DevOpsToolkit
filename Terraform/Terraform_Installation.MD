## Install Terraform on EC2 (Amazon Linux 2)

   ```sh
   sudo yum update –y 
   sudo yum install -y yum-utils
   sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
   sudo yum -y install terraform
   ```
## Test Terraform 

   ```sh
   terraform --version
   ```
