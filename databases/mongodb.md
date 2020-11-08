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
