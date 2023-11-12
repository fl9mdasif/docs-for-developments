# MongoDB Aggregation

## Aggregation is a way of processing a large number of documents is a collection by means of passing them through different stages.

### Today we are going to learn about the `$match, $addFields, $out, $merge, $$`

- `$match` Filters the documents to pass only the documents that match the specified condition(s) to the next pipeline stage.

```mongodb
db.practice_q.aggregate(
    [
        {$match:{ age: { $gt: 18 }, gender: "Male" }},
        {$project: {age:1}}
    ]
)
```

- `$addFields` Adds new fields to documents. $addFields outputs documents that contain all existing fields from the input documents and newly added fields.

```mongodb
db.practice_q.aggregate(
    [
        {$match: {gender:"Male"}},
        {$addFields: {course:"Next level dev 2", eduTech:"P-hero"} },
        {$project: {course:1,eduTech:1}}
    ]
)

```

- It doesn't modify the existing document with new filed. to do that we could use `$merge` operator to update existing document

```mongodb
db.practice_q.aggregate(
    [
        {$match: {gender:"Male"}},
        {$addFields: {course:"Next level dev 2", eduTech:"P-hero"} },
        {$project: {course:1,eduTech:1}},
        {$merge:"practice_q"}
    ]
)
```

- `$out` Takes the documents returned by the aggregation ⬆️⬆️ pipeline and writes them to a specified collection. means push data to a new collection.
- if there are project stage then just push these values into new collection

```mongodb
db.practice_q.aggregate(
    [
        {$match: {gender:"Male"}},
        {$addFields: {course:"Next level dev 2", eduTech:"P-hero"} },
        {$project: {naem:1,email:1,course:1,eduTech:1}},
        {$out:"Course-collection"} // new collection name
    ]
)
// so if we use $addFields then we should use the $out to return the new field with new collection into the db or use $merge to update doc

```

- The `$group` stage separates documents into groups according to a "group key".

This `$group, $count` counts the same different ages

```mongodb
db.practice_q.aggregate(
    [
        {$group:{_id:"$age", count:{$sum:1}}}
    ]
)


```

- `$push` push to a new field to return data

```mongodb

// push name to fullDoc

db.practice_q.aggregate(
    [
        {
            $group:
            {
                _id: "$address.country",
                count: { $sum: 1 },
                fullDoc: { $push: "$name" }
            },
        }
    ]
)

// $$ROOT means all documents
db.practice_q.aggregate(
    [
        {
            $group:
            {
                _id: "$address.country",
                count: { $sum: 1 },
                fullDoc: { $push: "$$ROOT" }
            },
           { $project: { "fullDoc.name": 1, "fullDoc.phone":1 } }
        }
    ]
)
```

```mongodb

```

```mongodb

```

```mongodb

```

```mongodb

```

```mongodb

```

```mongodb

```

```mongodb

```

```mongodb

```
