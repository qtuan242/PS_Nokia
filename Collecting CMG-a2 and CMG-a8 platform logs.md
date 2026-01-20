Collecting CMG-a2 and CMG-a8 platform logs

Commands used to collect data that can be used to troubleshoot the CMG-a2 and CMG-a8.

note-iconnote:
Some of the commands listed in Commands for collecting CMG-a8 platform logs may require root access privileges to the CMG-a system, which is not available to operators. To collect information using these commands, contact your local support organization.
It is also possible to collect most of the troubleshooting data described in this section using the cmg tech-report command.
Table: Commands for collecting CMG-a8 platform logs
Data	Command	Additional Information
###AIM information1

—	snmpget -v 2c -c AirFrameR 10.10.0.1 sysDescr.0	—
snmpget -v 2c -c AirFrameW 10.10.0.1 sysDescr.0	—
/etc/snmp/	Collect/display all logs in the directory
/var/log/aimmonitor.log	—
###Infrastructure information

—	cmg status	Display the status of the CMG VMs and system parameters
cmg health-status1	Display the status of the CMG-a server
ls -al /DELIV/2	—
ls -al /BACKUP/	—
ls -al /CMGax/	Where x is 2 or 8 for CMG-a2 or CMG-a8 respectively
df -al	—
###Infrastructure system logs

—	/var/log/cmg_rollback.log	—
/var/log/cmg-system-monitor.log-*	—
/var/log/cmg_update.log	—
/var/log/dmesg	—
/var/log/libvirt/qemu2	—
/var/log/messages-*2	—
/var/log/monitorLink*	Applicable only to CMG-a2
/var/log/yum.log2	—
/var/log/yum_update.log	—

###Airframe HW status logs
—	ipmitool sel info2	—
ipmitool sensor list2	—
ipmitool chassis status2	—
ipmitool fru print2	—
PCI devices	lspci -vvv3	Display the Kernel drivers and modules
—	lshw3	Display the list of PCIe devices
lscpu	—
ps -ax3	—
ps aux3	—
Driver information	modinfo ixgbe	Intel
Display the driver version, name, and PCI bus information

modinfo mlx5_core	Mellanox
Display the driver version, name, and PCI bus information

Kernel	uname --all	Display the Linux Kernel version
—	cat /etc/*release	—
Network infrastructure information
Linux bridge information	brctl show	Bridge to tap mappings
—	ip -s addr show dev ##	—
Interface settings	ethtool –g	—
ethtool –d2	—
ethtool bond03	—
Interface statistics	ethtool –S	—
ethtool –S bond0	—
ethtool –i	—
Host system information
—	cat /proc/cpuinfo	—
cat /proc/meminfo	—
cat /proc/cmdline	—
ps -ax | grep vhost | awk '{system("taskset -cp " $1)}'	—
Root logs
—	cd /root2	—
OpenStack version
—	rpm -qa	—
Host memory information
—	numastat	—
numastat -m	If available, may require additional tool package
Display the used and available memory per NUMA node

numastat -n	If available, may require additional tool package
numastat qemu2	If available, may require additional tool package
Display the NUMA memory used by the Hypervisor

numastat -pv2	Per-node process memory usage
mpstat -P ALL	—
vmstat -s	—
Host Virtual Machine Information
—	virsh dumpxml domain2	—
virsh vcpupin domain2	—
virsh domiflist domain2	—
virsh cpu-stats domain2	—
virsh version	—
virsh nodeinfo	—
virsh nodememstats	—
virsh list	—
virsh capabilities	—
DHCP System Logs4
—	cd /etc/dhcp2	—
/etc/dhcp/dhcpd6.conf2	—
/etc/dhcp/dhclient.d/ntp.sh2	—
/etc/dhcp/dhclient.d/chrony.sh2	—
/var/lib/dhcpd/dhcp*	—
/var/log/snmp/*	—
/var/lib/net-snmp/aim.conf	On both server 1 and 2
