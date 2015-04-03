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

