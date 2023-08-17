# MySQL Enterprise Backup
Create backup directory
```
mkdir /home/opc/backup-dir
mkdir /home/opc/backup-file
```
Run full backup using directory
```
mysqlbackup --user=root --host=127.0.0.1 --port=3306 --backup-dir=/home/opc/backup-dir --with-timestamp backup-and-apply-log
```
Run full backup using single FILE
```
mysqlbackup --user=root --host=127.0.0.1 --port=3306 --backup-dir=/home/opc/backup-file --backup-image=/home/opc/backup-file/full.mbi --with-timestamp backup-to-image
```
