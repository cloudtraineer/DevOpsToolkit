# Add Linux Host to Nagios Monitoring Server Using NRPE Plugin
we will show you how to add a Remote Linux machine and its services to the Nagios Core Monitoring host using NRPE (Nagios Remote Plugin Executor) agent.

## Installing Nagios Plugins and NRPE On Remote Linux Host
### Install Required Dependencies
```sh
yum install -y gcc glibc glibc-common gd gd-devel make net-snmp openssl-devel tar wget
```
### Create Nagios User
```sh
# useradd nagios
# passwd nagios
```
### Downlaod the Nagios Plugins
```sh
wget https://nagios-plugins.org/download/nagios-plugins-2.3.3.tar.gz
tar -xvf nagios-plugins-2.3.3.tar.gz
```
### Compile and Install Nagios Plugins
```sh
./configure 
make
make install

chown nagios.nagios /usr/local/nagios
chown -R nagios.nagios /usr/local/nagios/libexec
```
### Installing NRPE Plugin
```sh
wget https://github.com/NagiosEnterprises/nrpe/releases/download/nrpe-4.0.2/nrpe-4.0.2.tar.gz
tar xzf nrpe-4.0.2.tar.gz
cd nrpe-4.0.2
```
### Compile NRPE Plugin
```sh
./configure --disable-ssl
make all
make install-plugin
make install-daemon
make install-config
make install-init
```
### Configuring NRPE Plugin
Now open <b> /usr/local/nagios/etc/nrpe.cfg file </b> and add the local host and <b> IP address of the Nagios Monitoring Server </b>.
```sh
vi /usr/local/nagios/etc/nrpe.cfg file
allowed_hosts=127.0.0.1,<IP of Nagios host>

systemctl enable nrpe
systemctl restart nrpe
```
### Verify NRPE Daemon Locally
```sh
netstat -at | grep nrpe

Output:
tcp        0      0 0.0.0.0:nrpe            0.0.0.0:*               LISTEN     
tcp6       0      0 [::]:nrpe               [::]:*                  LISTEN 
```
verify the NRPE daemon is functioning properly by running the “check_nrpe”
```sh
/usr/local/nagios/libexec/check_nrpe -H 127.0.0.1

Output:
NRPE v4.0.2
```

## Adding Remote Linux Host to Nagios Monitoring Server

###
```sh
```

###
```sh
```
###
```sh
```
###
```sh
```
###
```sh
```
#
```sh
```
#
```sh
```

