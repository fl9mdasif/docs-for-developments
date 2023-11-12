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
           { $project:
                {
                    "fullDoc.name": 1,
                    "fullDoc.phone":1
                }
            }
        }
    ]
)
```

- `_id:null` means only one document which helps to calculate total `$sum`

```mongodb
db.practice_q.aggregate(
    [
        {
            $group: { _id: null ,
            totalSalary: { $sum: "$age" }
            }

        }
    ]
)


```

- `$max, $min, $avg, $subtract ` operator

```mongodb
db.practice_q.aggregate(
    [
        {
            $group: {
                _id: null,
                totalSalary: { $sum: "$salary" },
                maxSalary: { $max: "$salary" }, // $max operator
                minSalary: { $min: "$salary" }, // $min operator
                avgSalary: { $avg: "$salary" }, // $avg operator
            }
        },
        {
            $project: {
                totalSalary: 1,
                maxSalary: 1,
                minSalary: 1,
                averageSalary: "$avgSalary", // change field name
                subBetweenMaxAndMinSalary: { $subtract: [ "$maxSalary", "$minSalary"] }  // $subtract operator
            }
        }
    ]
)
```

- You cannot work directly on the elements of an array within a document with stages such as $group.
- The `$unwind` stage enables us to work with the values of the fields within an array.

```mongodb
db.practice_q.aggregate(
    [
        { $match: { email: "cpeaker5@nationalgeographic.com" } },
        {
            $unwind: "$skills" //
        }
    ]
)
```

```mongodb
db.practice_q.aggregate(
    [
        // stage 1
        {
            $unwind: "$friends" // breaks array operator to signle value
        },
        // stage 2
        {
            $group: { _id: "$friends", count:{$sum:1}}
        }
    ]
)

```

```mongodb
db.practice_q.aggregate(
    [
        { $unwind: "$interests" },
        {
            $group:
            {
                _id: "$age", interestPerAge: { $push: "$interests" }  // age er upor depends kore interest ber kora
            }
        }
    ]
)


```

- `$bucket` operator checks value between given value. crates a boundary

```mongodb
db.practice_q.aggregate(
    [
        {$bucket:{
            groupBy:"$age",  // creates groups
            boundaries:[20,40,60,80], // checks 0-19, 20-39, 40-59, 60-79 porjonto kotojon asd
            default:"greter than 80", // baki ja value ase
            output:{
                count:{$sum:1},
                seeTheirName:{$push:"$name"} // return the names of the ages
            }
        }}
    ]
)

```

- `$bucket` with `sort`, `$limit`

```mongodb
db.practice_q.aggregate(
    [
        // stage 1
        {
            $bucket: {
                groupBy: "$age",
                boundaries: [20, 40, 60, 80], // checks 0-19, 20-39, 40-59, 60-79 porjonto kotojon asd
                default: "greter than 80",
                output: {
                    count: { $sum: 1 },
                    seeTheirName: { $push: "$$ROOT" } // return the names of the ages
                }
            }
        },
        // stage 2
        { $sort: { count: 1 } }, //
        // stage 3 $limit used to return the limit of data
        { $limit: 3 }, // you must use the $limit stage just after the $sort
        // stage 4
        { $project: { count: 1 } }
    ]
)

```

```mongodb

```
