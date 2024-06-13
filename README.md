# OnMongoDB
MOngoDB is a NoSQL databases that stores data in flexible, JSON-like documents.

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

## Creating a Database

To create a database in MongoDB, we start by creating a `MongoCLient` object, then we specify a connection URL with the correct IP address and the name of the database we want to create.
<br>

```
import pymongo

myclient= pymongo.MongoClient("mongodb://localhost:27017?")

mydb=myclient["mydatabase"]
```
In MongoDB, a database is not created until it gets content!
<br>

MongoDB waits until you have created a collection (Table), with at least one document (record) before it actually creates the database (and collection).

## Check if Database Exists

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









