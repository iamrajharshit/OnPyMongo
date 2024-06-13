# MongoDB using Python
MOngoDB is a NoSQL databases that stores data in flexible, JSON-like documents.

## Contents
|SLno:|Topic|Description|
|--|--|--|
|0|[Introduction to PyMongo](#pymongo)||
|1|[Database in MongoDB](#python-mongodb-create-database)||
|2|[Collection in MongoDB](#python-mongodb-create-collection)||
|3|[Insert in MongoDB](#python-mongodb-insert-documant)||
|4|[Find in MongoDB](#python-mongodb-find)||
|5|[Query in MongoDB](#python-mongodb-query)||
|6|[Sort in MongoDB](#python-mongodb-sort)||
|7|[Delete in MongoDB](#python-mongodb-delete-document)||
|8|[Drop Collection in MongoDB](#python-mongodb-drop-collection)||
|9|[Update in MongoDB](#python-mongodb-update)||
|10|[Limit in MongoDB](#python-mongodb-limit)||

## PyMongo
Python can be used in database applications.
- Python needs a MongoDB driver to access the MongoDB database.
To install the MongoDB driver "PyMongo"

```
pip install pymongo
```
**Test PyMongo:**
<br> 

To test the installation, import pymongo and run the .py file.

```
import pymongo
```

## Python MongoDB Create Database

To create a database in MongoDB, we start by creating a `MongoCLient` object, then we specify a connection `URL` with the correct IP address and the name of the database we want to create.
<br>

```
import pymongo

myclient= pymongo.MongoClient("mongodb://localhost:27017/")

mydb=myclient["mydatabase"]
```
In MongoDB, a database is not created until it gets content!
<br>

MongoDB waits until you have created a collection (Table), with at least one document (record) before it actually creates the database (and collection).

### Check if Database Exists

Once the database is created, we check its existance. It can be done by listing all databases.

To return a list of databases.
```
print(myclient.list_database_name())
```
To check a specific database by name:

```
dblist=myclient.list_database_names()
if "mydatabase" in dblist:
    print("The database exists.")
```

## Python MongoDB Create Collection
A collection in MongoDB is the same as a table in SQL databases.

### Creating a collection
To create a collection in MongoDB, we use database object and spwcify the name of the collection you want to create.
<br>

To create a customers table in database;

```
import pymongo

myclient= pymongo.MongoClient("mongodb://localhost:27017/")

mydb=myclient["mydatabase"]
mycol = mydb["customers"]

```

**In MongoDB, a collection is not created until it gets content!** <br>
*MongoDB waits until you have inserted a document before it actually creates the collection.*

### To Check if Collection Exists
We can check the existance of a collection in a database by listing all the collections.

```
print(mydb.list_collection_names())
```

To check a specific collection by name:

```
collist=mydb.list_collection_names()
if "customers" in collist:
    print("The collection exists.")
```

## Python MongoDB Insert Documant

A document in MongoDB is the same as a record in SQL databases.

### Insert into collection
To insert a record, or document into a collection, we use the `insert_one()` method.
<br>
The first parameter of the `insert_one()` method is a dictionary containing the name(s) and value(s) of each field in the document you want to insert.

```
import pymongo

myclient = pytmongo.MongoClient("mongodb://localhost:27017")
mydb = myclient["mydatabase"]
mycol = mydb["customers"]

mydict={"name":"John","address":"Highway 37"}

x = mycol.insert_one(mydict)

```

### Return the id Field

The `insert_one()` method returns a InsertOneResult object, which has a property, `inserted_id` that holds the id of the inserted document.
<br>

Let's insert another record in the "customers" collection, and return the value of the `_id` field:
```
mydict = {"name":"Peter","address":"Lowstreet 27"}

x = mycol.insert_one(mydict)

print(x.inserted_id)

```
If you do not specify an `_id` field, then MongoDB will add one for you and assign a unique id for each document. <br>
Here, `_id` field is not specified, so MongoDB will assign a unique_id for the record (document).


### Insert Multiple Documents
To insert multiple documents into a collection in MongoDB, we use the `insert_many()` method. <br>
The first parameter of the `insert_many()` method is a list containing dictionaries with the data you want to insert:

```
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["mydatabase"]
mycol = mydb["customers"]

mylist= [


    {"name":"Amy","address":"Apple st 652"},
    {"name":"Harshit","address":"Mountaion 21"}
]

x = mycol.insert_many(mylist)

#print list of the _id values of the inserted documents:
print(x.inserted_ids)
```
The `insert_many()` method returns InsertManyResult object, which has a property, `insert_ids` that holds the ids of the inserted documents.

### Insert Multiple Docs with Specified IDs
We can assign unique ids by ourselves by specifying the `_id` field while inserting the document(s).<br>
The values has to be unique. Two documents cannot have same `_id`.

```
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["mydatabase"]
mycol = mydb["customers"]

mylist=[
    {"_id":1,"name":"John","address":"Highway 37"},
    {"_id":2,"name":"Rohan","address":"Highway 47"},
    {"_id":3,"name":"Mohan","address":"Highway 45"}
]

x = mycol.inset_many(mylist)

#print list of the _id values of the inserted documents
print(x.inserted_ids)

```
## Python MongoDB Find
MongoDB we use the find() and find_one() methods to find data in a collection. Just like the SELECT statement is used to find in a table in a MySQL database.<br>
### Find One
To select data from a collection in MongoDB, we can use the `find_one()` method.<br>
The `find_one()` method returns the first occurance in the selection.

```
import pymongo

myclient = pymongo.MongoClient("mongodb://localhoat:27017/")
mydb = myclient["mydatabase"]
mycol = mydb["customers"]

x =  mycol.find_one()

print(x)
```

### Find All
To select data from a table in MongoDB, we can also use the `find()` method as it returns all the occurrences in the selection.

```
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017")
mydb = myclient["mydatabase"]
mycol = mydb["customers"]

for x in mycol.find():
    print(x)
```
The first parameter of the find() method is a query object. Here we have not passed any parameter therefore, here it will show the same result as SELECT * in MySQL.

### Return Only Some Fields
The second parameter of the `find()` method is an object describing which fields to include in the result. <br>
This param is optional, and if kept empty, all fields will be included in the result.

```

import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017")
mydb = myclient["mydatabase"]
mycol = mydb["customers"]

for x in mycol.find({},{"_id":0,"name":1,"address":1}):
    print(x)

```

Its not allowed to specify both 0 and 1 values in the same object (except if one of the fields in the _id field). If we specify a field with the value 0, all other fields get the value 1 and vice versa.

```
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["mydatabase"]
mycol = mydb["customers"]

for x in mycol.find({},{"address":0}):
    print(x)
```

##### Error
**If we specify both 0 and 1 values in the same object (exept if one of the fields is the _id field):**

```
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["mydatabase"]
mycol = mydb["customers"]

for x in mycol.find({},{"name":1,"address":0}):
    print(x)

```

## Python MongoDB Query
### Filter the Result
When finding documents in a collection, you can filter the rsult by using a query object.<br>
The first argument of the `find()` method is a query object, and is used to limit the search.

```
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["mydatabse"]
mycol = mydb["customers"]

myquery = {"address": "Park Lane 38"}

mydoc = mycol.find(myquery)

for x in mydoc:
    print(X)
```

### Advance Query
To make advance queries we can use modifiers as values in the query object.

**Example: to find the doc where the "address" field stats with the letter "S" or higher (alpha), use the greater than modifier: {"$gt":"S"}**
```
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")

mydb = myclient["mydatabse"]

mycol = mycol["customers"]

myquery = {"address":{"$gt":"S"}}

mydoc = mycol.find(myquery)

for x in mydoc:
    print(X)

```

### Filter with Regular Expressions
Using regular expression as a modifier.<br>
Regular Expression can only be used with string datatype.

**To find only the documents where the "address" field starts with the letter "S", use the regular expression {"$regex": "^S"}:**

```
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017")

mydb = myclient["mydatabase"]
mycol = mydb["customers"]

myquery = {"address":{"$regex":"^S"}}

mydoc = mycol.find(myquery)

for x in mydoc:
    print(x)

```

## Python MongoDB Sort
### Sort the Result

### Sort Descending


## Python MongoDB Delete Document
### Delete Document

### Delete Many Documents

### Delete all Documents in a Collection



## Python MongoDB Drop Collection
### Delete Collection


## Python MongoDB Update
### Update Collection

### Update Many


## Python MongoDB Limit
### Limit the Result





