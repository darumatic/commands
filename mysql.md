# Connect to mysql server, database ddb

```
mysql --user=root --password=YOURPASS --host=10.20.14.247 --port 30290 ddb
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
