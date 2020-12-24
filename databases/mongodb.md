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

mongo "mongodb+srv://m001-student:admin@/sandbox.yfm6k.mongodb.net/admin"

mongo "mongodb+srv://sandbox.yfm6k.mongodb.net/maindb" --username m001-student
mongo "mongodb+srv://sandbox.yfm6k.mongodb.net/mdbu" --username m001-student

<!-- from the forum -->
mongo "mongodb+srv://sandbox.yfm6k.mongodb.net/test" --username m001-student --password m001-mongodb-basics

<!-- working -->
mongo "mongodb+srv://sandbox.yfm6k.mongodb.net/m001" --username m001-student


## Find command

show dbs

use sample_training

show collections

db.zips.find({"state": "NY"})

db.zips.find({"state": "NY"}).count()

db.zips.find({"state": "NY", "city": "ALBANY"})

db.zips.find({"state": "NY", "city": "ALBANY"}).pretty()


## insert

db.inspections.insert({
      "_id" : ObjectId("56d61033a378eccde8a8354f"),
      "id" : "10021-2015-ENFO",
      "certificate_number" : 9278806,
      "business_name" : "ATLIXCO DELI GROCERY INC.",
      "date" : "Feb 20 2015",
      "result" : "No Violation Issued",
      "sector" : "Cigarette Retail Dealer - 127",
      "address" : {
              "city" : "RIDGEWOOD",
              "zip" : 11385,
              "street" : "MENAHAN ST",
              "number" : 1712
         }
  })

db.inspections.insert({
      "id" : "10021-2015-ENFO",
      "certificate_number" : 9278806,
      "business_name" : "ATLIXCO DELI GROCERY INC.",
      "date" : "Feb 20 2015",
      "result" : "No Violation Issued",
      "sector" : "Cigarette Retail Dealer - 127",
      "address" : {
              "city" : "RIDGEWOOD",
              "zip" : 11385,
              "street" : "MENAHAN ST",
              "number" : 1712
         }
  })

db.inspections.find({"id" : "10021-2015-ENFO", "certificate_number" : 9278806}).pretty()

## insert ordereded

db.inspections.insert([{ "_id": 1, "test": 1 },{ "_id": 1, "test": 2 },
                       { "_id": 3, "test": 3 }],{ "ordered": false })

## Update

db.zips.updateMany({ "city": "HUDSON" }, { "$inc": { "pop": 10 } })

db.zips.updateOne({ "zip": "12534" }, { "$set": { "population": 17630 } })

db.grades.updateOne({ "student_id": 250, "class_id": 339 },
                    { "$push": { "scores": { "type": "extra credit",
                                             "score": 100 }
                                }
                     })

db.zips.updateMany({ "city": "HUDSON" }, { "$inc": { "pop": 10 } })

##

<!-- Delete all the documents that have test field equal to 1. -->

db.inspections.deleteMany({ "test": 1 })

db.inspections.deleteOne({ "test": 3 })

<!-- Drop the inspection collection. -->
db.inspection.drop()


## query operators

<!-- How many documents in the sample_training.zips collection have fewer than 1000 people listed in the pop field? -->
db.zips.find({ "pop": { "$lte" : 1000 }}).pretty()

<!-- What is the difference between the number of people born in 1998 and the number of people born after 1998 in the sample_training.trips collection? -->

db.trips.find({ "birth year": { "$gt": 1998 }}).count()
db.trips.find({ "birth year": 1998 }).count()