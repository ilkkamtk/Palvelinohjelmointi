# NoSQL MongoDB database and mongoose
#### Patrick Ausderau / Ilkka Kylmäniemi

### Study before the classes

* https://youtu.be/VOLeKvNz-Zo
* https://youtu.be/ofme2o29ngU

---

# NoSQL

* NoSQL = "non SQL", "non relational" or "not only SQL"
* Contains about anything that is not only relational databases
    * Document databases like MongoDB
    * Key-value storages like Redis and Memcached
    * Graph databases like OrientDB and Neo4j
    * Object databases like ObjectDB
    * Tabular databases like Hbase and BigTable

---

# MongoDB

* [Free & open source](https://github.com/mongodb/mongo) document-oriented database
    * First released in February 2009
    * Current version v7.x

---

# Main features

* Ad hoc queries
  * MongoDB supports field, range queries, regular expression searches
  * Queries can return specific fields of documents and also include user-defined JavaScript functions
* Indexing
  * Indexing means that you can define indexes on any field in the document to improve query performance. For example if you have a collection of users and you often query by email, you can create an index on the email field: 
  
    ```javascript
    db.collection.createIndex({ email: 1 })
    ```
  * The number 1 means that the index is ascending. You can also create a descending index with -1. After creating the index, you can query the collection like this:
      
     ```javascript
     db.collection.find({ email: 'frank@metropolia.fi' })
     ```
    
* High availability (replication)
   * Replication means that the data is copied to multiple servers. If one server goes down, the data is still available on the other servers. MongoDB can have multiple copies of data, which are called replicas. If the primary server goes down, one of the replicas is automatically promoted to the primary server. This is called failover.
* Supports sharding
   * The difference to replication is that sharding means that the data is split into multiple servers. This is called horizontal scaling. For example, if you have a collection of users and you have a lot of users, you can split the collection into multiple servers. This way you can handle more users. MongoDB automatically balances the data between the servers.
* File storage
   * Possible but not recommended for large files.
* JavaScript execution
   * JavaScript can be used in queries, aggregation, and map-reduce functions. For example map-reduce can be used to calculate the average of a field in a collection:
       
    ```javascript
    db.collection.mapReduce(
      () => emit(1, this.field),
      (key, values) => Array.avg(values),
      { out: 'average' }
    )
    ```

---

# Basic structure

* Server has multiple databases
* Database has multiple collections
    * Like tables in relational database
    * Usually contains many related documents
    * Do not enforce schema on documents
    * Starting from 3.2 validations available for documents (checked on inserts and updates)
* Collection has multiple documents (rows)
    * JSON-like structure for collection of key-value pairs
    * Serialized and stored as BSON (Binary JSON)
* Document has multiple fields (columns)
    * Can have different fields from each other within the collection
    * Collection-wide unique identifier (12-byte, stored usually in _<!--_-->id field) can be manually set for document. If not set, one is generated

---

# Modeling data

* Modeling the data is different from relational databases
    * Storage is cheap compared to computing time
    * Duplicating data is acceptable
* Model data by user requirements
* Joins are possible on write instead of read. This is called embedding
    * Easier to read but harder to update
    * Use references for large data sets. This is similar to foreign keys in relational databases
* Optimize for most usual use case
* Example: Blog post documents stores tags (strings) and comments (objects with text, author etc.) within the blog document itself (with author, text, date, etc.)

---

## MariaDB vs MongoDB

| **Criteria**                   | **MariaDB (Relational Database)**                            | **MongoDB (NoSQL Database)**                             |
|--------------------------------|-------------------------------------------------------------|----------------------------------------------------------|
| **Data Type**                  | Organized in tables (like spreadsheets).                    | Organized in flexible documents (like folders of files). |
| **Best For**                   | Structured, consistent data (e.g., customer databases).     | Flexible, changing data (e.g., social media posts).      |
| **Schema**                     | Fixed structure; data must fit into predefined tables.      | Flexible structure; data can vary from entry to entry.   |
| **Scalability**                | Grows by adding power to a single server.                   | Grows by adding more servers.                            |
| **Speed**                      | Fast for complex queries and transactions.                  | Fast for large volumes of simple data.                   |
| **Relationships**              | Great for connected data (e.g., sales and customers).       | Better for isolated or loosely connected data.           |
| **Examples**                   | Banking systems, inventory management.                     | Real-time analytics, content management systems.         |
| **Development**                | Requires careful planning; changes can be difficult.        | Easy to adapt; changes are simple to make.               |

- **Use MariaDB** when your data is structured, consistent, and requires complex operations (like finance or inventory systems).
- **Use MongoDB** when your data is flexible, unstructured, and grows rapidly (like social media or big data applications).

---



### Assignment 1
* [Create a free MongoDB Atlas cluster](https://youtu.be/YfyKoMNavs4) (or scroll down to the end of this page)
* [Install MongoDB Database tools (mongodump, mongorestore, mongoexport, mongoimport)](https://www.mongodb.com/docs/database-tools/installation/installation/). Download the tools [from here](https://www.mongodb.com/try/download/database-tools).
* Restore [this database](mongodump.zip) using `mongorestore` command: `mongorestore --uri mongodb+srv://<USERNAME>:<PASSWORD>@<HOST>`
* Browse the restored database in MongoDB Atlas
* Database has three collections: `categories`, `species`, and `animals`
   - `categories` contains the categories of species
   - `species` contains the species and they belong to a category
   - `animals` contains the animals and they belong to a species
* 'Pike' species has a placeholder image, replace it with a real image URL from wikipedia: `https://upload.wikimedia.org/wikipedia/commons/c/c6/Hecht_im_Sundh%C3%A4user_See.jpg`
* Here is a practical application that uses the database: [Animal App](https://ilkkamtk.github.io/)
---

## MongoDB CRUD operations

* [Create](https://docs.mongodb.com/manual/tutorial/insert-documents/) a new document
* [Read](https://docs.mongodb.com/manual/tutorial/query-documents/) documents
* [Update](https://docs.mongodb.com/manual/tutorial/update-documents/) documents
* [Delete](https://docs.mongodb.com/manual/tutorial/remove-documents/) documents

---

### Assignment 2
* Install [MongoDB Shell](https://docs.mongodb.com/mongodb-shell/) and connect to your MongoDB Atlas cluster: `mongosh "mongodb+srv://<HOST>" --apiVersion 1 --username <username>`
* [Basic commands](https://www.mongodb.com/docs/mongodb-shell/run-commands/)
* Select database `use palvelinohjelmointi`, show all databases: `show dbs`
* [Perform CRUD operations](https://www.mongodb.com/docs/mongodb-shell/crud/)
1. Select all documents from the `species` collection
2. Select all documents from the `animals` collection
* [Query and Projection Operators](https://www.mongodb.com/docs/manual/reference/operator/query/#query-and-projection-operators)
* [Date](https://www.mongodb.com/docs/manual/reference/method/Date/#insert-and-return-isodate-objects)
1. Select all animals born before 2020
2. Select all animals born in 2023
* [Geospatial queries](https://www.mongodb.com/docs/manual/geospatial-queries/)
1. Find all species in the following box (approx. Africa). Use the following coordinates:
   ```javascript
   [-26.484784820398147, -33.21557838270862], // Bottom left corner (Longitude, Latitude)
   [50.265511387965944, 32.1716316994946]  // Upper right corner (Longitude, Latitude)
    ```
2. Find all animals in 100km range from Helsinki. Use the following coordinates:
   ```javascript
   [24.9384, 60.1695] // Helsinki (Longitude, Latitude)
   ```
3. Find all animals in France. Use [geojson.io](https://geojson.io/) to draw a polygon around France and use those coordinates in the query. Make sure that the start and end coordinates are the same so that the polygon is closed.

---

## MongoDB Aggregation

* [Aggregation](https://docs.mongodb.com/manual/aggregation/) operations process data records and return computed results
* Aggregation operations group values from multiple documents together and can perform a variety of operations on the grouped data to return a single result
* Example: Calculate the average age of animals
  ```javascript
  db.animals.aggregate([
    {
      $group: {
        _id: null,
        avgAgeMs: { $avg: { $subtract: [new Date(), '$birthdate'] } }
      }
    },
    {
      $project: {
        avgAgeYears: { $divide: ['$avgAgeMs', 1000 * 60 * 60 * 24 * 365.25] }
      }
    }
    ])
  ```
* In the example above:
  1. First stage - `$group`
     * `_id: null`: Groups all documents into a single group.
     * `avgAgeMs`: Calculates the average age in milliseconds.
  2. Second stage - `$project`
     * `avgAgeYears`: Converts milliseconds to years by dividing by the number of milliseconds in a year.
* Example, get all species and their categories. Show only the species name and category name:
  ```javascript
  db.species.aggregate([
    {
      $lookup: {
        from: 'categories',
        localField: 'category',
        foreignField: '_id',
        as: 'category'
      }
    },
    {
      $unwind: '$category'
    },
    {
      $project: {
        _id: 1,
        species_name: 1,
        category_name: '$category.category_name'
      }
    }
  ])
  ```
---

## Assignment 3
* Select all cats from the `animals` collection and populate the `species` field to show also the species data. Use the following [stages](https://www.mongodb.com/docs/manual/reference/operator/aggregation-pipeline/):
   * Select `_id` from `species` collection where `species_name` is `Cat`. ($lookup)
   * Then use that `_id` to get all cats from `animals`. ($match)
   * Populate `species` field with the data from `species` collection ($unwind).
   * Show all data from `animals` including the species data ($project).
---

## Submitting assignments
Submit the queries you used in MongoDB shell to Oma. Don't submit any files, just write the queries in the text field.

---

#### Extra: Local installation:
* [Install](https://docs.mongodb.com/manual/administration/install-community/)
* [Shell access](https://docs.mongodb.com/mongodb-shell/) (eventually, some UI like [Studio 3T](https://studio3t.com/download-studio3t-free))
* [Enable authentication](https://blog.tericcabrel.com/enable-authentication-and-authorization-on-mongodb/)


---

## Sources

* [NoSQL (wikipedia)](https://en.wikipedia.org/wiki/NoSQL)
* [MongoDB](https://www.mongodb.com/)

---
