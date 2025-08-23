#  Install & configure Maven build tool on Jenkins
Maven is a code build tool which used to convert your code to an artifact. this is a widely used plugin to build in continuous integration


#### Prerequisites
1. Jenkins server

<!---  #### Install Maven on Jenkins
1. Download maven packages https://maven.apache.org/download.cgi onto Jenkins server.
 - Link : https://maven.apache.org/download.cgi
    ```sh
     sudo wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo
     sudo sed -i s/\$releasever/6/g /etc/yum.repos.d/epel-apache-maven.repo
     sudo yum install -y apache-maven
     ```
#### Checkpoint 
1. logoff and login to check maven version
  
    ```sh
    mvn --version
    ```
So far we have completed the installation of maven software to support maven plugin on the jenkins console. Let's jump onto Jenkins to complete the remaining steps. 
--->
### Setup maven on Jenkins console
1. Install maven plugin without restart  
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `Maven Integration`

2. Configure maven path
  - `Manage Jenkins` > `Global Tool Configuration` > `Maven`
```sh
# Reboot the Jenkins Server
```


### Setup JDK on Jenkins console (Optional in case of Error)
 ```sh
 # If this JDK configuration is missing or misconfigured, Maven won't be able to find it, resulting in the NullPointerException.
```
1. In Jenkins server, ensure that you have configured a JDK in the global tools configuration.
   - `Go to Manage Jenkins` > `Global Tool Configuration` > `Install JDK`


