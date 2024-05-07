# Installation, Setup and Configuration
## MySQL Server and MySQL Shell Installation
Extract MySQL Server binary
```
cd installer
unzip MySQL-Server-8.4.0.zip
```
Install MySQL Server
```
sudo yum localinstall -y mysql-commercial-*
```
Extract MySQL Shell
```
unzip MySQL-Shell-8.4.0.zip
```
Install MySQL Shell
```
sudo yum localinstall -y mysql-shell-commercial-*.rpm
```
Clean up
```
rm $HOME/*.rpm
```
## OS Parameter Tuning
Edit /etc/fstab using "sudo vi /etc/fstab" and change 
```
/dev/mapper/ocivolume-root /                       xfs     defaults        0 0
```
to
```
/dev/mapper/ocivolume-root /                       xfs     defaults,noatime,nodiratime     0 0
```
Reboot the VM.
```
sudo reboot
```
Fine tuning Linux Kernel
```
sudo vi /etc/sysctl.conf


fs.file-max=3248957
fs.aio-max-nr=1048576
net.core.netdev_max_backlog=8000
net.core.somaxconn=8000
net.ipv4.tcp_max_syn_backlog=4096
net.ipv4.ip_local_port_range=9000  65000
net.netfilter.nf_conntrack_tcp_timeout_time_wait=10
vm.swappiness=1
vm.dirty_ratio=10
vm.dirty_background_ratio=5
vm.dirty_expire_centisecs=500
vm.dirty_writeback_centisecs=100

net.core.rmem_default=262144
net.core.rmem_max=16777216
net.core.wmem_default=262144
net.core.wmem_max=16777216
net.ipv4.tcp_rmem=4096 87380 16777216
net.ipv4.tcp_wmem=4096 65536 16777216
net.ipv4.tcp_syn_retries=0
net.ipv4.tcp_synack_retries=0
net.ipv4.tcp_keepalive_time=30
net.ipv4.tcp_keepalive_intvl=1
net.ipv4.tcp_keepalive_probes=2

kernel.shmmax=30923764531
kernel.shmall=7549748
kernel.sem=32000 1024000000 500 32000
kernel.msgmax=65535
kernel.msgmnb=65535

```
Apply parameters
```
sudo sysctl -p
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
Configure MySQL Instance 3306
```
mysql -uroot -h::1

set persist_only innodb_redo_log_capacity=2147483648;
set persist_only innodb_flush_neighbors=2;
set persist_only innodb_io_capacity=3000;
set persist_only innodb_io_capacity_max=3000;
set persist_only innodb_buffer_pool_size=25769803776;
set persist_only innodb_buffer_pool_instances=12;
set persist_only innodb_lru_scan_depth=250;
set persist_only innodb_page_cleaners=12;
set persist_only innodb_checksum_algorithm=strict_crc32;
set persist_only binlog_row_image=MINIMAL;

set persist_only innodb_thread_sleep_delay=500; 
set persist_only innodb_spin_wait_delay=2;
set persist_only innodb_spin_wait_pause_multiplier=10;

set persist_only sql_generate_invisible_primary_key=on;

restart;

exit;
```
Download and install world_x schema/data
```
wget http://downloads.mysql.com/docs/world_x-db.zip

unzip world_x-db.zip

mysql -uroot -h::1 < world_x-db/world_x.sql

mysql -uroot -h::1 -e "show databases"
```
