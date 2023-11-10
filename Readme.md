# Mongoose Master doc

MongoDB is a no-sql database, which support both SQL, No-Sql. MongoDB is scalable, high performance, open source database.

We can install mongoDB to create database into our localhost,
To do that we have to install

- Install `MongoDB`
- install `MongoDB shell `

MongoDB Shell is the quickest way to connect to (and work with) MongoDB. Easily query data, configure settings, and execute other actions with this modern, extensible command-line interface — replete with syntax highlighting, intelligent autocomplete, contextual help, and error messages.

we have some option to write queries into

1. Mongosh shell in mongoDB compass
2. using our cmd (create a env variable into path C:\Program Files\MongoDB\Server\6.0\bin )
3. or using any GUI

we can also use GUI `Graphical User interface` like `No Sql Booster`

- install `noSqlBooster`

NoSQLBooster is a cross-platform GUI tool for MongoDB Server 3.6-7.0, which provides a build-in MongoDB script debugger, comprehensive server monitoring tools, chaining fluent query, SQL query, query code generator, task scheduling, ES2020 support, and advanced IntelliSense experience.

## Now we are able to write quires through the GUI.

### Let's learn about operators of mongodb with [Mongoose(mongodb shell) operator documentation ](https://www.mongodb.com/docs/manual/reference/method/db.collection.insert/)

There are `4 types of operators` in mongodb

1. Query and projection operator
2. Update operator
3. Aggregation pipeline
4. Bitwise Update operator

### Some basic queries: from Query and projection operator

- db.collection.insertOne({name:"Asif"}) // single obj
- db.collection.insertMany([{name:"Asif"}]) // array of ojb
- db.practice_q.find({}) // find all data
- db.practice_q.find({age:18}) // find if age = 18
- db.practice_q.find({age:18},{age:1}) // field filtering find if age = 18 and shows only the age `key/field/property` from data
- db.practice_q.find({age:18}).project({name:1, age:1}) // field filtering with project and shows only the name & age key from data

### 1.1 comparison Query operators `$gt, $gte, $lt, $lte`

- db.inventory.find( { age: { $gt: 20 } } ) // greeter than 20
- db.inventory.find( { age: { $gte: 20 } } ) // greeter than and equal 20
- db.inventory.find( { age: { $lt: 18 } } ) // less than 18
- db.inventory.find( { age: { $lte: 18 } } ) // less than 18

* db.practice_q.find({gender:"Female", age:{$gt:20,$lt:35}},{age:1}).sort({age:1}) //multiple condition

`$in & $nin` // checks if it is in or not in
To specify an $in expression, use the following prototype:
syntax: { field: { $in: [<value1>, <value2>, ... <valueN> ] } }

- db.inventory.find( { age: { $in: [18,20] } } ) // checks age field is either 18 or 20.
- db.inventory.find( { age: { $in: [18,26] } } ,{age:1}) // checks age field is either 18 or 20.

* db.practice_q.find({
  gender: "Female",
  age: { $in: [18, 24, 26, 32, 30] },
  interests: { $in: ["Cooking"] }}) // Implicit and

### 1.2 Logical query operators
