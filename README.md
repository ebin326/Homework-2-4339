CIS 4339 - Homework 2
=====================

Mongo DB Exercises (Individual assignment)
------------------------------------------

This is designed as tutorial and exercise model assignment. Each exercise is to be solved and reported. Each exercise builds on the last, so it’s best to work through these exercises in order.

Important

1.  Make sure to run everything in mongo shell. The `mongo` shell has been deprecated in MongoDB v5.0. The replacement is `mongosh`.
    
2.  It will be useful to refer to the `mongosh` usage documentation [https://docs.mongodb.com/mongodb-shell/run-commands/](https://docs.mongodb.com/mongodb-shell/run-commands/)
    

### Using Github Classroom to setup the remote repo for this assignment

To set up the remote github repository for your submission correctly, please follow the link posted in Canvas to accept the assignment via GitHub classroom.

You will then have an empty repository in GitHub.

### What to turn in

Create one file to store the code for the exercises, name it **Homework2.mongodb**. Write your exercise code in there, don’t forget to add comments to explain your code. Create a second file to record the outputs, called **Homework2\_output.txt**

Commit your source code (properly commented) to your private repository in the Github classroom. Do not zip the files together. Your github repository should show multiple meaningful commits illustrating how you worked through the exercises.

Submit the link to your github repository containing your homework via Canvas.

### Grading and feedback

We will start grading this assignment 2 weeks after initial assignment date. We will put a grade and feedback into Canvas. Your grade will not be final until the final deadline for this homework assignment. You can create new commits into the GitHub repository and res-submit via BB which will notify the instructor that you would like your assignment submission to be re-graded.

Let’s get started …​

Introduction
------------

Mongo DB is a highly scalable, NoSQL, schema free data store.

Data in MongoDB is in the format of JSON. JSON is JavaScript Object Notation that is used to create API’s and carry lightweight data. You can learn more about JSON [here](https://www.tutorialspoint.com/json/json_overview.htm).

Basics first.

### Starting the MongoDB server software

Start the MongoDB service on your local machine following instructions outlined at the end of installation. Starting the service will be differnt depending whether you are working on Windows, Mac OS etc. [https://docs.mongodb.com/manual/administration/install-community/](https://docs.mongodb.com/manual/administration/install-community/)). This tutorial assumes that you setup the MongoDB server without additional security measures (authorization) which is the default.

### Connecting to the MongoDB server

You need to connect to your MongoDB server.

You can simply use the MongoDB Compass tool to access the `mongosh` as it was demonstrated in class.

Or you can use a a terminal command to start up a mongoshell via `mongosh`. If you setup your MongoDB server with authentication use something like: `mongosh localhost:27017 -u username -p passw0rd`

### Tutorial Part A

#### Creating a Database

use tutorial

If the database named _tutorial_ does not exists, it will be automatically created.

The command `db` can be used to lookup the current database. After the `use` comand mongoshell "switches" to the selected DB and commands will operate on that DB.

#### List all Databases

show dbs

This will list all the databases. Although unless a collection is added, it won’t show.

#### Drop Database

db.dropDatabase()

This command will drop (delete) the current database.

### Exercise Part A

1.  Create a new database named _employees_ using the `use` command.
    
2.  List all DBs on your instance.
    

* * *

### Tutorial Part B

#### Collections

Data in MongoDB is stored in the form of documents. These documents are stored in a Collection. Collections are stored in a database.

A Collection can be created using the command:

db.createCollection('techdep');

#### List all the Collections in current database

show collections

### Exercise Part B

1.  Create a database named _temporary_, add a collection named _test_ to it, then drop the database. Report the output of `show dbs` and `show collections` before and after dropping the database.
    
2.  Create multiple collections using distinct department names in the database created in **exercise Part A**
    
3.  List all the collections in the current database
    

* * *

### Tutorial Part C - Documents

In MongoDB documents are basically JSON objects. JSON is used to carry lightweight data, so the maximum size limit for MongoDB document is 16mb, which is more than enough. MongoDB actually stores data records as BSON documents. BSON is a binary representation of JSON documents, though it contains more data types than JSON.

MongoDB documents are composed of field-and-value pairs and have the following structure:

{
   field1: value1,
   field2: value2,
   field3: value3,
   ...
   fieldN: valueN
}

The value of a field can be any of the BSON data types, including other documents, arrays, and arrays of documents. For example, the following document contains values of varying types:

var mydoc = {
               \_id: ObjectId("5099803df3f4948bd2f98391"),
               name: { first: "Alan", last: "Turing" },
               birth: new Date('Jun 23, 1912'),
               death: new Date('Jun 07, 1954'),
               contribs: \[ "Turing machine", "Turing test", "Turingery" \],
               views : NumberLong(1250000)
            }

The above fields have the following data types:

`_id_` _holds an \_ObjectId_.  
`name` holds an embedded document that contains the fields `first` and `last`.  
`birth` and death hold values of the _Date_ type.  
`contribs` holds an _array_ of _strings_.  
`views` holds a value of the _NumberLong_ type.  

The field name `_id` is reserved for use as a primary key; its value must be unique in the collection, is immutable, and may be of any type other than an array. If the `_id` contains subfields, the subfield names cannot begin with a (`$`) symbol.

You can consult the [MongoDB documentation](https://docs.mongodb.com/manual/core/document/) to learn more about the MongoDB document types.

#### Inserting documents

The format for a basic insert command is as follows: db.collection\_name.insert()

Here is an example.

db.techdep.insert(
{
    employeeid: 56,
    name: "John Parker",
    salary: 70000
})

The _insert_ method inserts a document in the given collection name and if the collection does not exist, it creates a new collection with the given name. if you don’t define an `_id` filed, it will be automatically created.

#### Retrieving documents

You can find a document or documents matching a particular pattern using the `find` function.

db.collection\_name.find()

### Exercise Part C

1.  Insert some employee documents with fields(employeeid, name, salary) in the multiple collections created in the previuos exercise.
    
2.  Use the find command to retrieve all the entered data.
    

* * *

### Tutorial Part D - Query Documents

`find()` can be used to query the documents by applying simples rules, which include using:

*   ID (ObjectId)
    
*   Strings like email, first name, last name etc.
    
*   Using comparision operations like gt(greater than), ls(less than) etc..
    
*   Using regular expressions
    

More complex querying can be done using aggregation (not part of this tutorial).

#### Finding by ID

The `_id` field of any collection is automatically indexed.

Therefore, if the `_id` of the document is know, the following command can be used:

db.collection\_name.find(ObjectId("584723948hjhuyg34"))

IDs are 12 byte BSON objects, not Strings which is why we need to use the ObjectId data type.

#### Finding using field name:

db.collection\_name.find({
    field\_name: value
    field\_name: value
        .
        .
})

One or multiple field\_name parameters can be used for finding the document.

Example,

db.techdep.find({
    salary: 70000,
    name: "John Parker"
})

Regular Expression a.k.s regex can be used as:

db.techdep.find({
    name: "/pattern/"
})

Please consult [https://www.guru99.com/regular-expressions-mongodb.html](https://www.guru99.com/regular-expressions-mongodb.html) to learn more about Regex patterns in MongoDB queries.

### Exercise Part D

1.  Add the follwoing employees to any collection:
    
    db.collection\_name.insert({
        employeeid: 1187,
        name: "John Steight",
        salary: 75000
    })
    db.collection\_name.insert({
        employeeid: 2455,
        name: "Syed",
        salary: 90000
    })
    db.collection\_name.insert({
        employeeid: 24113,
        name: "Wright John",
        salary: 65000
    })
    
2.  Find 2 documents using the field name **employeeid**
    
3.  Find 2 documents using the field name **name**
    
4.  Find all the documents where the name field starts with John
    
5.  Find all the documents where the name filed contains John
    

* * *

### Tutorial Part E - Querying using comparison operations

We can find documents using comparison querying as well.

General syntax:

db.collection\_name.find({
    field\_name: {
        $gt: value // that is an example of a comparison operator
    }
})

This above command can be used to find documents greater than the given value.

For instance:

db.collection\_name.find({
    salary: {
        $gt: 70000
    }
})

We can also use operators like:

*   $lt(less than)
    
*   $exists(The field exists)
    
*   $gte(greater than equals)
    
*   $lte(less than equals)
    

The complete list can be found [here](https://docs.mongodb.com/manual/reference/operator/query/).

### Exercise Part E - Working with Real World Data

We are going to use real world data: [https://media.mongodb.org/zips.json](https://media.mongodb.org/zips.json)

First, download the file onto your computer. Your task is to import that data into a new collection called **zips**.

You need to install the MongoDB Tools [https://docs.mongodb.com/database-tools/installation/installation/](https://docs.mongodb.com/database-tools/installation/installation/) if you have not done so already in order to be able to use the `mongoimport` command in a terminal.

Use the following command in a terminal (may need to close the mongosh command):

mongoimport --db employees --collection zips --file zips.json

Then open the mongoshell again, execute the following tasks and report code and output:

1.  Get all cities with a _population of less than 1500_
    
2.  Find all data for the city _CHESTER_ using query operator
    
3.  Use array query operator to find entries for the location at _\-84.38570799999999, 45.015207_
    
4.  Use a logical operator to find all entries that match the city WARREN or location -80.76424299999999, 41.231819
    

* * *

### Tutorial Part F - The $where operator

**$Where** can be used to match documents that satisfy a JavaScript expression. A string containing a JavaScript expression or a JavaScript function can be passed using the $where operator. The JavaScript expression or function may be referred as `this` or `obj`.

For example,

db.collection\_name.find( { $where: function() { return (this.name == "John") }})

### Exercise Part F

1.  Use the $where operator to find all employess who have a salary > 75000
    
2.  Use the $where operator to find employees where the name contains John
    

### Tutorial Part G - Projection

Projection is used to limit the output results of a query. The `find()` method takes a second parameter which allows to whitelist required fields.

db.collection\_name.find({},{
    field\_name: true/false,
        .
        .
    })

### Exercise Part G

1.  Using only one collection from the employees database query all the documents in that collection but return only name and salary.
    

#### Basic Grading rubric for this homework

 

| Item  | Exercise Part A |
| :------:|:----:|
| Item | Points |
|Exercise Part A|10|
|Exercise Part B|15|
|Exercise Part C|10|
|Exercise Part D|25|
|Exercise Part E|25|
|Exercise Part F|10|
|Exercise Part G|5|
|Total|100|
|Deduction 50% if code throsw errors|(up to -50 points)|
|Deduction for not submitting link to Github in Canvas|-5 points|
|Deduction for not having multiple meaningful commits showing progress|-10 points|
