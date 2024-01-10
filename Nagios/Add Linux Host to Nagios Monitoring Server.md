# Add Linux Host to Nagios Monitoring Server Using NRPE Plugin
we will show you how to add a Remote Linux machine and its services to the Nagios Core Monitoring host using NRPE (Nagios Remote Plugin Executor) agent.

## Installing Nagios Plugins and NRPE On Remote Linux Host
### Install Required Dependencies
```sh
yum install -y gcc glibc glibc-common gd gd-devel make net-snmp openssl-devel tar wget
```
### Create Nagios User
```sh
useradd nagios
passwd nagios
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

## Installing NRPE On Nagios Monitoring Server

###  Install NRPE Plugin
```sh
wget https://github.com/NagiosEnterprises/nrpe/releases/download/nrpe-4.0.2/nrpe-4.0.2.tar.gz
tar xzf nrpe-4.0.2.tar.gz
cd nrpe-4.0.2
```

### Compile and install the NRPE addon.
```sh
./configure --disable-ssl
make all
make install-plugin
make install-daemon
make install-init
```
### Verify NRPE Daemon Remotely
Make sure that the check_nrpe plugin can communicate with the NRPE daemon on the remote Linux host. Add the IP address in the command below with the IP address of your Remote Linux host
```sh
/usr/local/nagios/libexec/check_nrpe -H <remote_linux_ip_address>

Output:
NRPE v4.0.2
```
## Adding Remote Linux Host to Nagios Monitoring Server

###  Creating Nagios Host and Services File
```sh
cd /usr/local/nagios/etc/
touch hosts.cfg
touch services.cfg
```
Now add these two files to the main Nagios configuration file. Open the nagios.cfg file with any editor.
```sh
 vi /usr/local/nagios/etc/nagios.cfg
```
Now add the two newly created files as shown below.
```sh
# You can specify individual object config files as shown below:
cfg_file=/usr/local/nagios/etc/hosts.cfg
cfg_file=/usr/local/nagios/etc/services.cfg
```

### Configuring Nagios Host and Services File
Now open hosts.cfg file and add the default host template name and define remote hosts as shown below. Make sure to replace host_name, alias, and address with your remote host server details.
```sh
vi /usr/local/nagios/etc/hosts.cfg
```
```sh
## Default Linux Host Template ##
define host{
name                            linux-box               ; Name of this template
use                             generic-host            ; Inherit default values
check_period                    24x7        
check_interval                  5       
retry_interval                  1       
max_check_attempts              10      
check_command                   check-host-alive
notification_period             24x7    
notification_interval           30      
notification_options            d,r     
contact_groups                  admins  
register                        0                       ; DONT REGISTER THIS - ITS A TEMPLATE
}

## Default
define host{
use                             linux-box               ; Inherit default values from a template
host_name                       tecmint		        ; The name we're giving to this server
alias                           CentOS 6                ; A longer name for the server
address                         <IP Address>            ; IP address of Remote Linux host
}
```
Next open services.cfg file and add the following services to be monitored.
```sh
vi /usr/local/nagios/etc/services.cfg
```
```sh
define service{
        use                     generic-service
        host_name               tecmint
        service_description     CPU Load
        check_command           check_nrpe!check_load
        }

define service{
        use                     generic-service
        host_name               tecmint
        service_description     Total Processes
        check_command           check_nrpe!check_total_procs
        }

define service{
        use                     generic-service
        host_name               tecmint
        service_description     Current Users
        check_command           check_nrpe!check_users
        }

define service{
        use                     generic-service
        host_name               tecmint
        service_description     SSH Monitoring
        check_command           check_nrpe!check_ssh
        }

define service{
        use                     generic-service
        host_name               tecmint
        service_description     FTP Monitoring
        check_command           check_nrpe!check_ftp
        }
```

### Configuring NRPE Command Definition
Now NRPE command definition needs to be created in commands.cfg file.
```sh
vi /usr/local/nagios/etc/objects/commands.cfg
```
Add the following NRPE command definition at the bottom of the file.
```sh
###############################################################################
# NRPE CHECK COMMAND
#
# Command to use NRPE to check remote host systems
###############################################################################

define command{
        command_name check_nrpe
        command_line $USER1$/check_nrpe -H $HOSTADDRESS$ -c $ARG1$
        }
```
# verify Nagios Configuration files for any errors
```sh
/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```
restart Nagios to apply recent configuration changes
```sh
systemctl restart nagios
```
