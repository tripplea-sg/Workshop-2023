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
Edit /etc/fstab "/dev/mapper/ocivolume-root /  xfs     defaults    0 0" to </br>
/dev/mapper/ocivolume-root /                       xfs     defaults        0 0
