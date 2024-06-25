
### Basic Commands

1. **Connecting to MongoDB**:
   ```shell
   mongo
   ```

2. **Show Databases**:
   ```js
   show dbs
   ```

3. **Switch to a Database**:
   ```js
   use databaseName
   ```

4. **Show Collections**:
   ```js
   show collections
   ```

### CRUD Operations

#### Create

1. **Insert a Single Document**:
   ```js
   db.collectionName.insertOne({ key1: "value1", key2: "value2" })
   ```

2. **Insert Multiple Documents**:
   ```js
   db.collectionName.insertMany([
     { key1: "value1", key2: "value2" },
     { key1: "value3", key2: "value4" }
   ])
   ```

#### Read

1. **Find All Documents**:
   ```js
   db.collectionName.find()
   ```

2. **Find with a Condition**:
   ```js
   db.collectionName.find({ key: "value" })
   ```

3. **Find One Document**:
   ```js
   db.collectionName.findOne({ key: "value" })
   ```

4. **Find with Projection (Selecting Fields)**:
   ```js
   db.collectionName.find(
     { key: "value" },
     { key1: 1, key2: 1 }
   )
   ```

5. **Count Documents**:
   ```js
   db.collectionName.countDocuments({ key: "value" })
   ```

#### Update

1. **Update One Document**:
   ```js
   db.collectionName.updateOne(
     { key: "value" },
     { $set: { keyToUpdate: "newValue" } }
   )
   ```

2. **Update Multiple Documents**:
   ```js
   db.collectionName.updateMany(
     { key: "value" },
     { $set: { keyToUpdate: "newValue" } }
   )
   ```

3. **Replace a Document**:
   ```js
   db.collectionName.replaceOne(
     { key: "value" },
     { key1: "newValue1", key2: "newValue2" }
   )
   ```

#### Delete

1. **Delete One Document**:
   ```js
   db.collectionName.deleteOne({ key: "value" })
   ```

2. **Delete Multiple Documents**:
   ```js
   db.collectionName.deleteMany({ key: "value" })
   ```

### Advanced Queries

#### Sorting

1. **Sort Results**:
   ```js
   db.collectionName.find().sort({ key: 1 }) // 1 for ascending, -1 for descending
   ```

#### Pagination

1. **Limit the Number of Results**:
   ```js
   db.collectionName.find().limit(10)
   ```

2. **Skip Documents**:
   ```js
   db.collectionName.find().skip(5)
   ```

#### Aggregation

1. **Aggregation Pipeline**:
   ```js
   db.collectionName.aggregate([
     { $match: { key: "value" } },
     { $group: { _id: "$key2", total: { $sum: "$key3" } } },
     { $sort: { total: -1 } }
   ])
   ```
Certainly! MongoDB's Aggregation Framework is a powerful tool for data processing and transformation. It allows you to perform complex queries and data manipulations in a pipeline format. Here are the main aggregation stages and operators you should know:

### Aggregation Pipeline

An aggregation pipeline consists of stages, each performing an operation on the input documents and passing the results to the next stage.

```js
db.collectionName.aggregate([
   { /* stage1 */ },
   { /* stage2 */ },
   ...
])
```

### Common Aggregation Stages

1. **$match**
   - Filters documents to pass only those that match the specified condition(s).
   ```js
   { $match: { key: "value" } }
   ```

2. **$group**
   - Groups documents by a specified key and performs aggregate operations on the grouped data.
   ```js
   { 
     $group: { 
       _id: "$key", 
       total: { $sum: "$key2" } 
     } 
   }
   ```

3. **$project**
   - Reshapes each document in the stream, such as by including, excluding, or adding fields.
   ```js
   { 
     $project: { 
       key1: 1, 
       key2: 1, 
       newField: { $sum: ["$key3", "$key4"] } 
     } 
   }
   ```

4. **$sort**
   - Sorts the documents based on a specified field(s).
   ```js
   { $sort: { key: 1 } } // 1 for ascending, -1 for descending
   ```

5. **$limit**
   - Limits the number of documents passed to the next stage.
   ```js
   { $limit: 10 }
   ```

6. **$skip**
   - Skips the specified number of documents.
   ```js
   { $skip: 5 }
   ```

7. **$unwind**
   - Deconstructs an array field from the input documents to output a document for each element.
   ```js
   { $unwind: "$arrayField" }
   ```

8. **$lookup**
   - Performs a left outer join to a different collection in the same database.
   ```js
   {
     $lookup: {
       from: "otherCollection",
       localField: "key",
       foreignField: "otherKey",
       as: "newArrayField"
     }
   }
   ```

9. **$addFields**
   - Adds new fields to documents.
   ```js
   { 
     $addFields: { 
       newField: "value", 
       anotherField: { $sum: ["$key1", "$key2"] }
     } 
   }
   ```

10. **$replaceRoot**
    - Replaces the input document with the specified document.
    ```js
    { $replaceRoot: { newRoot: "$newDoc" } }
    ```

11. **$out**
    - Writes the resulting documents to a specified collection.
    ```js
    { $out: "newCollection" }
    ```

12. **$merge**
    - Merges the output of the aggregation pipeline into a specified collection.
    ```js
    {
      $merge: {
        into: "targetCollection",
        whenMatched: "merge", // options: "replace", "merge", "keepExisting"
        whenNotMatched: "insert" // options: "insert", "discard"
      }
    }
    ```

### Common Aggregation Operators

1. **$sum**
   - Calculates the sum of numeric values.
   ```js
   { $group: { _id: null, total: { $sum: "$field" } } }
   ```

2. **$avg**
   - Calculates the average of numeric values.
   ```js
   { $group: { _id: null, average: { $avg: "$field" } } }
   ```

3. **$min**
   - Finds the minimum value.
   ```js
   { $group: { _id: null, minValue: { $min: "$field" } } }
   ```

4. **$max**
   - Finds the maximum value.
   ```js
   { $group: { _id: null, maxValue: { $max: "$field" } } }
   ```

5. **$push**
   - Creates an array of values from input documents.
   ```js
   { $group: { _id: null, items: { $push: "$field" } } }
   ```

6. **$addToSet**
   - Creates an array of unique values.
   ```js
   { $group: { _id: null, uniqueItems: { $addToSet: "$field" } } }
   ```

7. **$first**
   - Returns the first value in a group of documents.
   ```js
   { $group: { _id: null, firstValue: { $first: "$field" } } }
   ```

8. **$last**
   - Returns the last value in a group of documents.
   ```js
   { $group: { _id: null, lastValue: { $last: "$field" } } }
   ```

9. **$arrayElemAt**
   - Returns the element at the specified array index.
   ```js
   { $arrayElemAt: ["$array", index] }
   ```

10. **$concatArrays**
    - Concatenates arrays.
    ```js
    { $concatArrays: ["$array1", "$array2"] }
    ```

11. **$filter**
    - Filters elements of an array.
    ```js
    {
      $filter: {
        input: "$array",
        as: "item",
        cond: { $gt: ["$$item.field", value] }
      }
    }
    ```

12. **$map**
    - Applies a function to each element in an array.
    ```js
    {
      $map: {
        input: "$array",
        as: "item",
        in: { newField: "$$item.oldField" }
      }
    }
    ```

### Example Aggregation Pipeline

To demonstrate how these stages and operators can work together, here’s an example pipeline that matches documents, groups them, and sorts the results:

```js
db.collectionName.aggregate([
  { $match: { status: "A" } },
  { $group: { _id: "$cust_id", total: { $sum: "$amount" } } },
  { $sort: { total: -1 } }
])
```

In this example:
- `$match` filters documents where `status` is "A".
- `$group` groups documents by `cust_id` and calculates the total `amount`.
- `$sort` sorts the results in descending order of `total`.

Understanding and practicing these stages and operators will help you handle a variety of data processing tasks in MongoDB.

### Indexing

1. **Create an Index**:
   ```js
   db.collectionName.createIndex({ key: 1 })
   ```

2. **Create a Unique Index**:
   ```js
   db.collectionName.createIndex({ key: 1 }, { unique: true })
   ```

### Common Query Operators

1. **Comparison Operators**:
   ```js
   db.collectionName.find({ key: { $eq: value } }) // equal to
   db.collectionName.find({ key: { $ne: value } }) // not equal to
   db.collectionName.find({ key: { $gt: value } }) // greater than
   db.collectionName.find({ key: { $gte: value } }) // greater than or equal to
   db.collectionName.find({ key: { $lt: value } }) // less than
   db.collectionName.find({ key: { $lte: value } }) // less than or equal to
   db.collectionName.find({ key: { $in: [value1, value2] } }) // in array
   db.collectionName.find({ key: { $nin: [value1, value2] } }) // not in array
   ```

2. **Logical Operators**:
   ```js
   db.collectionName.find({ $and: [ { key1: value1 }, { key2: value2 } ] })
   db.collectionName.find({ $or: [ { key1: value1 }, { key2: value2 } ] })
   db.collectionName.find({ key: { $not: { $gt: value } } })
   db.collectionName.find({ key: { $nor: [ { key1: value1 }, { key2: value2 } ] } })
   ```

3. **Element Operators**:
   ```js
   db.collectionName.find({ key: { $exists: true } })
   db.collectionName.find({ key: { $type: "string" } })
   ```

4. **Evaluation Operators**:
   ```js
   db.collectionName.find({ key: { $mod: [divisor, remainder] } }) // modulus
   db.collectionName.find({ key: { $regex: /pattern/ } }) // regex
   db.collectionName.find({ $where: "this.key == 'value'" }) // JavaScript expression
   ```

5. **Array Operators**:
   ```js
   db.collectionName.find({ key: { $all: [value1, value2] } }) // all values in array
   db.collectionName.find({ key: { $elemMatch: { subKey: subValue } } }) // matches element in array
   db.collectionName.find({ key: { $size: value } }) // size of array
   ```

These are the foundational queries and commands you need to know for MongoDB. During an interview, you may be asked to perform specific tasks or solve problems using these commands. Make sure you understand the syntax and the context in which each command is used. Practice writing and executing these queries to build your confidence.