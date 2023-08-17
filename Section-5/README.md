# MySQL Enterprise Security
## Transparent Data Encryption (TDE)
Install plugin on 3306, 3307, 3308, 5506, 2206
```
vi /home/opc/mysql-sandboxes/<port>/my.cnf

## content
early-plugin-load=keyring_encrypted_file.so
keyring_encrypted_file_data=/home/opc/mysql-sandboxes/<port>/sandboxdata/keyring-encrypted
keyring_encrypted_file_password=password
```
Restart 3306, 3307, 3308, 5506, 2206 one at a time
```
mysql -uroot -h::1 -P<port> -e "restart"
```
Check PRIMARY cluster
```
mysqlsh gradmin:grpass@localhost:3306 -- cluster status
```
Restart Cluster for 5506
```
mysqlsh gradmin:grpass@localhost:5506 -- dba rebootClusterFromCompleteOutage
```
Check current keyring installation status
```
mysql -uroot -h::1 -e "show plugins" | grep keyring_encrypted_file
mysql -uroot -h::1 -P3307 -e "show plugins" | grep keyring_encrypted_file
mysql -uroot -h::1 -P3308 -e "show plugins" | grep keyring_encrypted_file
mysql -uroot -h::1 -P2206 -e "show plugins" | grep keyring_encrypted_file
mysql -uroot -h::1 -P5506 -e "show plugins" | grep keyring_encrypted_file
```
Gather table data from OS
```
strings -a /home/opc/mysql-sandboxes/3306/sandboxdata/world_x/countryinfo.ibd
strings -a /home/opc/mysql-sandboxes/3307/sandboxdata/world_x/countryinfo.ibd
strings -a /home/opc/mysql-sandboxes/3308/sandboxdata/world_x/countryinfo.ibd
strings -a /home/opc/mysql-sandboxes/2206/sandboxdata/world_x/countryinfo.ibd
strings -a /home/opc/mysql-sandboxes/5506/sandboxdata/world_x/countryinfo.ibd
```
Encrypt table using TDE
```
mysql -uroot -h::1 -e "alter table world_x.countryinfo encryption='Y'"
```
Gather table data from OS
```
strings -a /home/opc/mysql-sandboxes/3306/sandboxdata/world_x/countryinfo.ibd
strings -a /home/opc/mysql-sandboxes/3307/sandboxdata/world_x/countryinfo.ibd
strings -a /home/opc/mysql-sandboxes/3308/sandboxdata/world_x/countryinfo.ibd
strings -a /home/opc/mysql-sandboxes/2206/sandboxdata/world_x/countryinfo.ibd
strings -a /home/opc/mysql-sandboxes/5506/sandboxdata/world_x/countryinfo.ibd
```
Select data from MySQL
```
mysql -uroot -h::1 -e "select * from world_x.countryinfo limit 1"
mysql -uroot -h::1 -P3307 -e "select * from world_x.countryinfo limit 1"
mysql -uroot -h::1 -P3308 -e "select * from world_x.countryinfo limit 1"
mysql -uroot -h::1 -P2206 -e "select * from world_x.countryinfo limit 1"
mysql -uroot -h::1 -P5506 -e "select * from world_x.countryinfo limit 1"
```
