# Add user to database

```
use mydb
db.createUser(
   {
     user: "accountUser",
     pwd: "password",
     roles: [ "readWrite", "dbAdmin" ]
   }
)
```

# Restoring a database dump

```
eg. to restore to the mydb database you need this folder structure:

home
    dump
        mydb
            (dump bson and json files ...)

then in the home folder execute 

mongorestore -u $USER -p $PASSWORD -h $HOST
```

# Drop a database

```
mongo -u user -p pass your_mongodb_ip/admin
> use mydb
switched to db mydb
> db.dropDatabase()
{ "dropped" : "mydb", "ok" : 1 }

```


