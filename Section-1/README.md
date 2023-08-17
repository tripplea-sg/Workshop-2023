# Installation, Setup and Configuration
## MySQL Installation
Extract MySQL Server binary
```
unzip MySQL-server-8.1-01.zip
```
Install MySQL Server
```
sudo yum localinstall -y mysql-commercial-*
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
