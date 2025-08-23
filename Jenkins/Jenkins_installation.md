# Install Jenkins on AWS EC2
Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Mac OS X, and other Unix-like operating systems. As an extensible automation server, Jenkins can be a simple CI server or a continuous delivery hub for any project.

### Prerequisites
1. EC2 Instance 
   - With Internet Access
   - Security Group with Port `8080` open for internet
2. Java 11 should be installed  


## Install Jenkins on EC2 (Amazon Linux 2)
 You can install jenkins using the rpm or by setting up the repo. We will set up the repo to update it easily in the future.
   Get the latest version of jenkins from https://pkg.jenkins.io/redhat-stable/ and install
   ```sh
   sudo yum update –y 
   sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
   sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
   sudo amazon-linux-extras install epel
   sudo amazon-linux-extras install java-openjdk11 -y   OR   sudo yum install java-17-amazon-corretto -y 
   sudo yum install jenkins -y
   ```
## Install Jenkins on EC2 (Amazon Linux 2023)
 You can install jenkins using the rpm or by setting up the repo. We will set up the repo to update it easily in the future.
   Get the latest version of jenkins from https://pkg.jenkins.io/redhat-stable/ and install
   ```sh
   sudo yum update –y 
   sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
   sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
   sudo dnf install java-11-amazon-corretto -y  OR sudo dnf install java-17-amazon-corretto-devel -y
   sudo yum install jenkins -y
   ```
   You need to increase the size of "tmpfs" location
   ```sh
   df -h
   ```
   ![alt text](https://github.com/cloudtraineer/Installation_guide/blob/master/Jenkins/df.png?raw=true)
   ```sh
   # To increase the space on /tmp location follow the steps
   vi /etc/fstab
   
   # Paste the below line at the end of the file opened in pervious step
   tmpfs   /tmp         tmpfs   nodev,nosuid,size=2G          0  0
  
   # Save the file and Restart the server
   # Once the Server is rebooted verify the size has been increased to 2 GB
   ```
   ![alt text](https://github.com/cloudtraineer/Installation_guide/blob/master/Jenkins/df_new.png?raw=true)

## Configure Jenkins
### Start Jenkins
   ```sh
   # Start jenkins service
   service jenkins start

   # Setup Jenkins to start at boot,
   chkconfig jenkins on
   ```
### Setting up Java JDK
```sh
# Add or Update Java Home path in Environment
sudo systemctl edit jenkins
Environment="JAVA_HOME=/usr/lib/jvm/java-17-amazon-corretto"
    
# Reload Jenkins Service
sudo systemctl daemon-reexec
sudo systemctl restart jenkins
```
### Accessing Jenkins
   By default jenkins runs at port `8080`, You can access jenkins at
   ```sh
   http://YOUR-SERVER-PUBLIC-IP:8080
   ```

#### Configure Jenkins
- The default Username is `admin`
- Grab the default password 
- Password Location: `/var/lib/jenkins/secrets/initialAdminPassword`
- `Install Suggested Plugin` Plugin Installation;
- Change the admin password
   - `Admin` > `Configure` > `Password`
- Configure `Java` path (Mandatory)
  - `Manage Jenkins` > `Global Tool Configuration` > `JDK`
  - `Click Add JDK` > `Give it a name (example: jdk17)` > `Set JAVA_HOME`
     ```sh
     /usr/lib/jvm/java-17-amazon-corretto
     ```
  - Uncheck Install automatically

### Test Jenkins Jobs
1. On Jenkins Dashboard click on “new item”
2. Enter an item name – `My-First-Project`
   - Chose `Freestyle` project
3. Under the Build section <br>
   - Choose the option 'Execute shell'
   - Enter the command: echo "Welcome to Jenkins Demo"
4. Save your job 
5. Click on 'Build Now' to execute the job. 
6. Check "console output"

### Test Jenkins Jobs to print date and time

1. On Jenkins Dashboard click on “new item”
2. Enter an item name – `My-Project`
   - Chose `Freestyle` project
3. Under the Build section <br>
   - Choose the option 'Execute shell'
   - Enter the command: echo $(date)
4. Save your job 
5. Click on 'Build Now' to execute the job. 
6. Check "console output"

<br>Note: Trigger the job multiple times to see the date and time 
