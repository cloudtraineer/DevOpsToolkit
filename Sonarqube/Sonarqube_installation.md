# Sonarqube Setup

SonarQube is an open-source static testing analysis software, that is used by developers to manage source code quality and consistency.
## 🧰 Prerequisites

Source: https://docs.sonarqube.org/latest/requirements/requirements/
1. An EC2 instance with a minimum of 2 GB RAM (t2.small)  
1. Java 11 installation   
   ```sh 
   amazon-linux-extras list
   amazon-linux-extras install java-openjdk11
   ```
1. SonarQube cannot be run as root on Unix-based systems, so create a dedicated user account for SonarQube if necessary.

## Installation steps

1. Download SonarQube [latest verions](https://www.sonarqube.org/downloads/) on to EC2 instance 
   ```sh 
   cd /opt  
   wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-x.x.zip  
   ```
1. extract packages
   ```sh 
   unzip /opt/sonarqube-x.x.zip
   ```

2. Change ownershipt to the user and Switch to the Linux binaries directory to start the service
   ```bash
   chown -R <sonar_user>:<sonar_user_group> /opt/sonarqube-x.x  
   cd /opt/sonarqube-x.x/bin/linux-x86-64   
   ./sonar.sh start
   ```
3. Connect to the SonarQube server through the browser. It uses port 9000.   
   `Note`: Port should be opened in the Security group 
   ```bash
   http://<Public-IP>:9000
   ```
# Integrate Sonarqube with Jenkins 

## 🧰 Prerequisites
1. a Sonarqube Server
2. A Jenkins server

Steps :- 
### On Sonarqube server 

1. Generate a sonarqube token to authenticate from Jenkins

### On Jenkins server 

1. Install Sonarqube plugin
2. Configure Sonarqube credentials
3. Add Sonarqube to jenkins "configure system"
4. Install SonarScanner
5. Run Pipeline job
