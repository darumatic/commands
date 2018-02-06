# Connect to mysql server, database ddb

```
mysql --user=root --password=YOURPASS --host=10.20.14.247 --port 30290 ddb
```

# Show databases

```
mysql> SHOW databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| ddb                |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.11 sec)
```

# Show tables

```
mysql> SHOW TABLES
    -> 
    -> 
    -> ;
Empty set (0.01 sec)

```

# Create database dump and restore it

```
  mysqldump -h hostname -u user --password=password databasename > filename 
  mysql -h hostname -u user --password=password databasename < filename
```
