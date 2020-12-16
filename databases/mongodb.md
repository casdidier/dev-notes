# Finding the current database you’re in

`db

# Listing databases

`show databases

# 3. Go to a particular database

`use <your_db_name>

# 4. Creating a Database

`use <your_db_name> ( if exist go to the db if not create one)

# 5.insert new data in collection

`db.myCollection.insert({"name": "john", "age" : 22, "location": "colombo"})

`db.createCollection("myCollection")

`db.createCollection("mySecondCollection", {capped : true, size : 2, max : 2})

## export / import

mongodump --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"

mongoexport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --collection=sales --out=sales.json

mongorestore --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"  --drop dump

mongoimport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --drop sales.json

## connect with shell

mongo "mongodb+srv://<username>:<password>@<cluster>.mongodb.net/admin"

mongo "mongodb+srv://m001-student:admin@/sandbox.yfm6k.mongodb.net.mongodb.net/admin"

mongo "mongodb+srv://sandbox.yfm6k.mongodb.net/maindb" --username m001-student
mongo "mongodb+srv://sandbox.yfm6k.mongodb.net/mdbu" --username m001-student
mongo "mongodb+srv://sandbox.yfm6k.mongodb.net/m001" --username m001-student

## Find command

show dbs

use sample_training

show collections

db.zips.find({"state": "NY"})

db.zips.find({"state": "NY"}).count()

db.zips.find({"state": "NY", "city": "ALBANY"})

db.zips.find({"state": "NY", "city": "ALBANY"}).pretty()