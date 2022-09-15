
# MongoDB Shell Note

## Command

### Run external script

	mongo load('path/script.js')

## Backup

### Dump db

	mongodump -d dbName


### Restore db

	mongorestore -d NewDBName --directoryperdb dumppDBPath


## Query

### Show all indexes

	db.collection.getIndexes()

### Find record has no field

	db.collection.find( {uid: null} )

### Find by operator

```
$lt - '<'
$lte - '<='
$gte - '>='
$ne - '!='
$in - 'is in array'
$nin - '! in array'
```

```
db.collections.find({a: {'$in': [2, 3, 4]}});
db.collections.find({a: {'$gte': 2, '$lte': 4}});
db.collections.find({num: {'$gt': 15}});
```

### Sort by date

	> db.collection.find().sort({date: 1})

### Show record clearly
	
	> db.collection.find().toArray()

### Change field name

```
obj.xid = obj.id
db.collection.save(a)
```

### Delete a field

	> delete(obj.id)

### Push and pull items from arrays

```
> db.users.update({name: 'Hank'}, {'$pull': {'languages': 'scala'} });
> db.users.update({name: 'Hank'}, {'$push': {'languages': 'javascript'} }); 
```

### Clear Document

```
> db.collections.remove();
> db.user.remove({name: 'Sue'})
```

## Other

### Displays all the databases on the server you are connected

	show dbs

### Show current using db
	db

### Drop db

```
> use databaseName
> db.dropDatabase()
```

### Show current db info

	db.serverStatus().host

### Show all collections

	show collections

### web admin interface

- [http://localhost:28017](http://localhost:28017)

### cursor-behaviors

The cursorInfo command returns information about current cursor allotment and use.

	> db.runCommand({cursorInfo:1})

[http://docs.mongodb.org/manual/core/read-operations/#cursor-behaviors](http://docs.mongodb.org/manual/core/read-operations/#cursor-behaviors)


### List collection data

	db.collections.find()

### Basic query

	db.collections.find({a: 2})

### Save document to collection

```
> db.collections.save({a: 99})
> db.users.save({name: 'Johnny', languages: ['ruby', 'c']})
> db.users.save({name: 'Sue', languages: ['scala', 'lisp']})
```

### Updates Document

```
> db.users.update({name: 'Johnny'}, {name: 'Cash', languages: ['english']}});
> db.users.update({name: 'Cash'}, {'$set': {'age': 50} }); 
```

### drop collection

	db.collection.drop()
