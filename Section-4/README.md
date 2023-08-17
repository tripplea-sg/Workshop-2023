# MySQL High Availability
## InnoDB Cluster
Create instance 3308
```
mysqlsh -e "dba.deploySandboxInstance(3308)"

mysql -uroot -h::1 -P3308

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
Configure instance
```
mysqlsh -- dba configure-instance { --host=127.0.0.1 --port=3306 --user=root } --clusterAdmin=gradmin --clusterAdminPassword='grpass' --interactive=false --restart=true

mysqlsh -- dba configure-instance { --host=127.0.0.1 --port=3307 --user=root } --clusterAdmin=gradmin --clusterAdminPassword='grpass' --interactive=false --restart=true

mysqlsh -- dba configure-instance { --host=127.0.0.1 --port=3308 --user=root } --clusterAdmin=gradmin --clusterAdminPassword='grpass' --interactive=false --restart=true

```
Create InnoDB Cluster
```
mysqlsh gradmin:grpass@localhost:3306 -- dba create-cluster mycluster
```
Tuning InnoDB Cluster
```
mysql -uroot -h::1

set persist group_replication_paxos_single_leader=on;
set persist_only group_replication_compression_threshold=100;
set persist group_replication_poll_spin_loops=100;

restart;
exit
```
Start InnoDB Cluster
```
mysqlsh gradmin:grpass@localhost:3306 -- dba rebootClusterFromCompleteOutage

mysqlsh gradmin:grpass@localhost:3306 -- cluster status
```
Add instance
```
mysqlsh gradmin:grpass@localhost:3306 -- cluster add-instance gradmin:grpass@localhost:3307 --recoveryMethod=incremental

mysqlsh gradmin:grpass@localhost:3306 -- cluster add-instance gradmin:grpass@localhost:3307 --recoveryMethod=clone
```






