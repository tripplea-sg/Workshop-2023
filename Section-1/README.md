# Installation, Setup and Configuration
## MySQL Server and MySQL Shell Installation
Extract MySQL Server binary
```
unzip MySQL-server-8.1-01.zip
```
Install MySQL Server
```
sudo yum localinstall -y mysql-commercial-*
```
Extract MySQL Shell
```
unzip MySQL-Shell-8.1.zip
```
Install MySQL Shell
```
sudo yum localinstall -y mysql-shell-commercial-8.1.0-1.1.el8.x86_64.rpm
```
## OS Parameter Tuning
Edit /etc/fstab using "sudo vi /etc/fstab" and change 
```
/dev/mapper/ocivolume-root /                       xfs     defaults        0 0
```
to
```
/dev/mapper/ocivolume-root /                       xfs     defaults,nodiratime,noatime      0 0
```
Reboot the VM.
```
sudo reboot
```
Check vm.swappiness
```
cat /proc/sys/vm/swappiness
```
Change vm.swappiness to "1" by open sysctl.conf
```
sudo vi /etc/sysctl.conf
```
Add the following line, save and exit
```
vm.swappiness=1
```
Reboot the VM.
```
sudo reboot
```
Check IO Scheduler
```
cat /sys/block/sda/queue/scheduler
```
Change IO Scheduler to none
```
sudo su
echo none > /sys/block/sda/queue/scheduler
exit
cat /sys/block/sda/queue/scheduler
```
## Setup and Configure Instance 3306
Create Sandbox Instance 3306
```
sudo systemctl stop mysqld
sudo systemctl disable mysqld
mysqlsh -e "dba.deploySandboxInstance(3306)"

## press enter for empty root password

```

