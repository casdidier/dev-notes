## connect with shell

mongo "mongodb+srv://<username>:<password>@<cluster>.mongodb.net/admin"

<!-- from the forum working -->
mongo "mongodb+srv://sandbox.yfm6k.mongodb.net/test" --username m001-student --password m001-mongodb-basics

<!-- working -->
mongo "mongodb+srv://sandbox.yfm6k.mongodb.net/m001" --username m001-student

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

<!-- Find all documents where airplanes CR2 or A81 left or landed in the KZN airport: -->

db.routes.find({ "$and": [ { "$or" :[ { "dst_airport": "KZN" },
                                    { "src_airport": "KZN" }
                                  ] },
                          { "$or" :[ { "airplane": "CR2" },
                                     { "airplane": "A81" } ] }
                         ]}).pretty()

<!-- How many businesses in the sample_training.inspections dataset have the inspection result "Out of Business" and belong to the "Home Improvement Contractor - 100" sector? -->

db.inspections.find({ "result": "Out of Business",
                      "sector": "Home Improvement Contractor - 100" }).count()


<!-- Which is the most succinct query to return all documents from the sample_training.inspections collection where the inspection date is either "Feb 20 2015", or "Feb 21 2015" and the company is not part of the "Cigarette Retail Dealer - 127" sector? -->
db.inspections.find(
  { "$or": [ { "date": "Feb 20 2015" },
             { "date": "Feb 21 2015" } ],
    "sector": { "$ne": "Cigarette Retail Dealer - 127" }}).pretty()



db.zips.find({ "pop": { "$gt": 5000, "$lt": 1000000 }}).count()
db.zips.find({ "$nor": [ { "pop": { "$lt":5000 } },
             { "pop": { "$gt": 1000000 } } ] } ).count()


<!-- How many companies in the sample_training.companies dataset were either founded in 2004 and either have the social category_code or web category_code, or were founded in the month of October and also either have the social category_code or web category_code? -->

db.companies.find().pretty()


## Expressive Query Operator

db.companies.find({ "$nor": [ { "pop": { "$lt":5000 } },
             { "pop": { "$gt": 1000000 } } ] } ).count()


<!-- Find all documents where the trip started and ended at the same station: -->
db.trips.find({ "$expr": { "$eq": [ "$end station id", "$start station id"] }
              }).count()

<!-- Find all documents where the trip lasted longer than 1200 seconds, and started and ended at the same station: -->
db.trips.find({ "$expr": { "$and": [ { "$gt": [ "$tripduration", 1200 ]},
                         { "$eq": [ "$end station id", "$start station id" ]}
                       ]}}).count()

<!-- find all the companies that have more employees than the year in which they were founded? -->
db.companies.find(
  { "$expr": { "$lt": [ "$founded_year", "$number_of_employees" ]}}).count()



db.companies.find(
  { "$expr": { "$eq": [ "$permalink", "$twitter_username" ]}}).count()


db.companies.find({"$expr":{"$eq":["$permalink","$twitter_username"]}}).count()

<!-- array operator -->

<!-- Find all documents with exactly 20 amenities which include all the amenities listed in the query array: -->

db.listingsAndReviews.find({ "amenities": {
                                  "$size": 20,
                                  "$all": [ "Internet", "Wifi",  "Kitchen",
                                           "Heating", "Family/kid friendly",
                                           "Washer", "Dryer", "Essentials",
                                           "Shampoo", "Hangers",
                                           "Hair dryer", "Iron",
                                           "Laptop friendly workspace" ]
                                         }
                            }).pretty()



db.listingsAndReviews.find({ "reviews": { "$size":50 },
                             "accommodates": { "$gt":6 }})



<!-- Using the sample_airbnb.listingsAndReviews collection find out how many documents have the "property_type" "House", and include "Changing table" as one of the "amenities"? -->
db.listingsAndReviews.find({ "amenities": {
                                  "$all": ["Changing table", "Free parking on premises", "Air conditioning", "Wifi"],
                                         },
                                        "bedrooms": 2
                            }).pretty()

