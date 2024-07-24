### How to Backup MySQL Database in Linux?

To take a backup of **MySQL** databases or databases,  the database must exist in the database server and you must have access  to it. The format of the command would be.

```bash
$ mysqldump -u "username" –p"password" "database_name" > dump_file.sql
```



#### How to backup MySQL Database with compression

If your mysql database is very big, you might want to compress your sql file. Just use the mysql backup command below and pipe the output to gzip, then you will get the output as gzip file.

```bash
$ mysqldump -u username -h localhost -p database_name | gzip -9 > backup_db.sql.gz
```



#### How to Backup Multiple MySQL Databases?

If you want to take backup of multiple databases, run the following  command. The following example command takes a backup of databases [**rsyslog**, **syslog**] structure and data into a single file called **rsyslog_syslog.sql**.

```bash
$ mysqldump -u"root" -p"tecmint" --databases rsyslog syslog > rsyslog_syslog.sql
```



#### How to Backup All MySQL Databases?

If you want to take a backup of all databases, then use the following command with the option **–all-database**. The following command takes the backup of all databases with their structure and data into a file called **all-databases.sql**.

```bash
$ mysqldump -u"root" -p"tecmint" --all-databases > all-databases.sql

```



#### How to Backup MySQL Database Structure Only?

If you only want the backup of the database structure without data, then use the option **–no-data** in the command. The below command exports database [**rsyslog**] **Structure** into a file **rsyslog_structure.sql**.

```bash
$ mysqldump -u"root" -p"tecmint" -–no-data rsyslog > rsyslog_structure.sql
```



#### How to Backup a Single Table of Database?

With the below command you can take a backup of a single table or  specific tables of your database. For example, the following command  only takes a backup of the **wp_posts** table from the database **wordpress**.

```bash
$ mysqldump -u root -ptecmint databses_name table_wp_posts > wordpress_posts.sql
```



#### How to Backup Multiple Tables of Database?

If you want to take a backup of multiple or certain tables from the database, then separate each table with space.

```bash
$ mysqldump -u root -ptecmint db_wordpress tbl_wp_posts tbl_wp_comments > wordpress_posts_comments.sql
```



#### How to Backup Remote MySQL Database

The below command takes the backup of the remote server [**172.16.25.126**] database [**gallery**] into a local server.

```bash
$ mysqldump -h 172.16.25.126 -u root -ptecmint gallery > gallery.sql
```



### How to Restore MySQL Database?

In the above tutorial, we have seen how to take the backup of  databases, tables, structures, and data only, now we will see how to  restore them using the following format.l

```bash
$ mysql -u "username" –p"password" "database_name" < "dump_file.sql"
```



If you want to restore a database that already exists on the targeted machine, then you will need to use the **mysqlimport** command.

```bash
$ mysqlimport -u root -ptecmint rsyslog < rsyslog.sql
```