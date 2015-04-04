# setup vmware workstation and set up hadoop cluster

Install the centos6.4 mimimul version as VM1 -> master
Use bridged adapter to share the network. If the Workstation add vnet to VMs, we can manually change the bridge adapter setting to use the ethernet of the physical machine.  EDIT->Virtual Network Editor

sudo vi /etc/sysconfig/network-scripts/ifcfg-eth0

IPADDR=10.0.0.100
NETMASK=255.255.255.0


sudo vi /etc/sysconfig/network
NETWORKING=yes
HOSTNAME=master.akcl.com 
GATEWAY=10.0.0.1


/etc/init.d/network restart

vi /etc/sysctl.conf

net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.all.disable_ipv6 = 1  
then run the following command
sysctl -p

Now we can clone this machines to create workers (slaves). I have done this. But encounterd an issue with MAC addresses. When we clone, the MAC address also will be same. (You can refresh the MAC adress in the advanced network features)

delete this file - /etc/udev/rules.d/70-persistant-net.rules
after restart, the new MAC address should be copied to 
/etc/sysconfig/network-scripts/ifcfg-eth0
HWADDR=

Property change the IPs and reboot the cloned machines. 


# On Each machine
adduser arun
passwd arun
visudo
root    ALL=(ALL)       ALL
arun    ALL=(ALL)       ALL

passwd root

vi /etc/ssh/sshd_config

PermitRootLogin no


Disable the IP Tables, so that the daemons on remote machines can talk to ports on the system

sudo service iptables stop

sudo chkconfig iptables off
Disable SELinux
sudo /usr/sbin/setenforce 0

sudo sed -i.old s/SELINUX=enforcing/SELINUX=disabled/ /etc/selinux/config



sudo vi /etc/hosts

10.0.0.100 master.akcl.com  master
10.0.0.101 worker1.akcl.com worker1
10.0.0.102 worker2.akcl.com worker2
10.0.0.103 worker3.akcl.com  worker3
10.0.0.104 worker4.akcl.com  worker4



To configure your system to use a proxy

On Red Hat systems, add the following property to /etc/yum.conf:



sudo vi /etc/yum.conf
proxy=http://server:port/



/etc/yum.repos.d/

http://archive.cloudera.com/cm4/redhat/6/x86_64/cm/cloudera-manager.repo

[cloudera-manager]
# Packages for Cloudera Manager, Version 4, on RedHat or CentOS 6 x86_64
name=Cloudera Manager
baseurl=http://archive.cloudera.com/cm4/redhat/6/x86_64/cm/4/
gpgkey = http://archive.cloudera.com/cm4/redhat/6/x86_64/cm/RPM-GPG-KEY-cloudera    
gpgcheck = 1


http://www.cloudera.com/content/cloudera/en/documentation/cloudera-manager/v4-latest/Cloudera-Manager-Version-and-Download-Information/Cloudera-Manager-Version-and-Download-Information.html
download cloudera-manager-installer.bin

chmod u+x cloudera-manager-installer.bin

sudo ./cloudera-manager-installer.bin



Point your web browser to http://master.akcl.com:7180/. Log in to Cloudera Manager with the username and    │
 │ password set to 'admin' to continue installation. (Note that the hostname may be incorrect. If the url does │
 │ not work, try the hostname you use when remotely connecting to this machine.) If you have trouble           │
 │ connecting, make sure you have disabled firewalls, like iptables.  
 
 
 
 http://master.akcl.com:7180
 
 Username: admin Password: admin
 
 service cloudera-scm-server restart
 
 
 
 
root    ALL=(ALL)       ALL
arun    ALL=(ALL)      NOPASSWD: ALL




