# Nagios Installation 
Nagios is an excellent open-source monitoring tool that provides comprehensive monitoring and alerting services. We will guide you through the process of installing Nagios on Amazon Linux, step by step.

### Install Prerequisite Software
```sh
sudo yum install httpd php -y
sudo yum install gcc glibc glibc-common -y
sudo yum install gd gd-devel -y
```
### Create Account Information
You need to set up a Nagios user. Run the following commands:
```sh
sudo adduser -m nagios
sudo passwd nagios
```
Type the new password twice.
```sh
sudo groupadd nagcmd
sudo usermod -a -G nagcmd nagios
sudo usermod -a -G nagcmd apache
```

### Download Nagios Core and the Plugins
Download the source code tarball of both Nagios and the Nagios plugins. over the period below download link may expire, so better to download it from the official Nagios link. (Visit http://www.nagios.org/download/ for links to the latest versions)
```sh
wget http://prdownloads.sourceforge.net/sourceforge/nagios/nagios-4.4.6.tar.gz
wget https://nagios-plugins.org/download/nagios-plugins-2.3.3.tar.gz
```

### Compile and Install Nagios
Extract the Nagios source code tarball.
```sh
tar zxvf nagios-4.4.6.tar.gz
cd nagios-4.4.6
```
Run the configuration script with the name of the group which you have created in the above step.
```sh
./configure --with-command-group=nagcmd
```
Compile the Nagios source code.
```sh
make all
```
Install binaries, init script, sample config files, and set permissions on the external command directory.
```sh
sudo make install
sudo make install-init
sudo make install-config
sudo make install-commandmode
```
### Customize Configuration
Change the E-Mail address with nagiosadmin contact definition you’d like to use for receiving Nagios alerts.
```sh
sudo vim /usr/local/nagios/etc/objects/contacts.cfg
```

### Configure the Web Interface
```sh
sudo make install-webconf
```
Create a nagiosadmin account for logging into the Nagios web interface. Note the password you need while login in to Nagios web console.

```sh
sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
sudo service httpd restart
```
### Compile and Install the Nagios Plugins
Extract the Nagios plugin’s source code tarball.
```sh
tar zxvf nagios-plugins-2.3.3.tar.gz
cd nagios-plugins-2.3.3
```
Compile and install the plugins.
```sh
./configure --with-nagios-user=nagios --with-nagios-group=nagios
make
sudo make install
```

### Start Nagios
Add Nagios to the list of system services and have it automatically start when the system boots.
```sh
sudo chkconfig --add nagios
sudo chkconfig nagios on
```
Verify the sample Nagios configuration files.
```sh
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```
If there are no errors, start Nagios.
```sh
sudo service nagios start
```

### Update AWS Security Group
You need to open <b>port 80 </b>on the new AWS EC2 server to incoming traffic so you can connect to the new Nagios webpage.

### Log in to the Web Interface
Access the Nagios web interface to do this you will need to know the Public DNS or IP for your instance, you can get this from the Instance section of the EC2 Console if you do not already know it.<br>
<b>You’ll be prompted for the username (nagiosadmin) and password you specified earlier. </b><br>
e.g. http://<Public_IP>/nagios/
