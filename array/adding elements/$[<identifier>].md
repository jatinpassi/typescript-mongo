## The filtered positional operator $[<identifier>] identifies the array elements that match the arrayFilters conditions for an update operation.

{ <update operator>: { "<array>.$[<identifier>]" : value } }, { arrayFilters: [ { <identifier>: <condition> } ] }
    
    Eg:
    db.collection.updateMany(
      { <query conditions> },
      { <update operator>: { "<array>.$[<identifier>]" : value } },
      { arrayFilters: [ { <identifier>: <condition> } ] }
    )

    Note: 
    1) Upsert: If an upsert operation results in an insert, the query must include an exact equality match on the array field in order to use $[<identifier>] in the update statement.
    db.collection.update(
      { myArray: [ 0, 1 ] },
      { $set: { "myArray.$[element]": 2 }},
      { arrayFilters: [ { element: 0 } ], 
      upsert: true }
    )
    changes: { "_id" : ObjectId(...), "myArray" : [ 2, 1 ] }
    2) If the upsert operation did not include an exact equality match and no matching documents were found to update, the upsert operation would error.
    db.array.update(
      { },
      { $set: { "myArray.$[element]": 10 } },
      { arrayFilters: [ { element: 9 } ],
        upsert: true }
    )
    3) Nested Arrays: The filtered positional operator $[<identifier>] can be used for queries which traverse more than one array and nested arrays.

# Update All Array Elements That Match arrayFilters

{ 
  "_id" : 1, 
  "grades" : [ 95, 92, 90 ] 
}
{ 
  "_id" : 2, 
  "grades" : [ 98, 100, 102 ] 
}
{ 
  "_id" : 3, 
  "grades" : [ 95, 110, 100 ] 
}

db.students.update({ },{ $set: { "grades.$[element]" : 100 } },{ multi: true,arrayFilters: [ { "element": { $gte: 100 } } ]})

{ 
  "_id" : 1, 
  "grades" : [ 95, 92, 90 ] 
}
{ 
  "_id" : 2, 
  "grades" : [ 98, 100, 100 ] 
}
{ 
  "_id" : 3, 
  "grades" : [ 95, 100, 100 ] 
}

# Update All Documents That Match arrayFilters in an Array
{
   "_id" : 1,
   "grades" : [
      { "grade" : 80, "mean" : 75, "std" : 6 },
      { "grade" : 85, "mean" : 90, "std" : 4 },
      { "grade" : 85, "mean" : 85, "std" : 6 }
   ]
}
{
   "_id" : 2,
   "grades" : [
      { "grade" : 90, "mean" : 75, "std" : 6 },
      { "grade" : 87, "mean" : 90, "std" : 3 },
      { "grade" : 85, "mean" : 85, "std" : 4 }
   ]
}

db.students2.update({ },{ $set: { "grades.$[elem].mean" : 100 } },{multi: true,arrayFilters: [ { "elem.grade": { $gte: 85 } } ]})

{
   "_id" : 1,
   "grades" : [
      { "grade" : 80, "mean" : 75, "std" : 6 },
      { "grade" : 85, "mean" : 100, "std" : 4 },
      { "grade" : 85, "mean" : 100, "std" : 6 }
   ]
}
{
   "_id" : 2,
   "grades" : [
      { "grade" : 90, "mean" : 100, "std" : 6 },
      { "grade" : 87, "mean" : 100, "std" : 3 },
      { "grade" : 85, "mean" : 100, "std" : 4 }
   ]
}

# Update All Array Elements that Match Multiple Conditions

{
   "_id" : 1,
   "grades" : [
      { "grade" : 80, "mean" : 75, "std" : 6 },
      { "grade" : 85, "mean" : 100, "std" : 4 },
      { "grade" : 85, "mean" : 100, "std" : 6 }
   ]
}
{
   "_id" : 2,
   "grades" : [
      { "grade" : 90, "mean" : 100, "std" : 6 },
      { "grade" : 87, "mean" : 100, "std" : 3 },
      { "grade" : 85, "mean" : 100, "std" : 4 }
   ]
}

db.students2.update({ },{ $inc: { "grades.$[elem].std" : -1 } },{ arrayFilters: [ { "elem.grade": { $gte: 80 }, "elem.std": { $gt: 5 } } ], multi: true })

{  "_id" : 1,
   "grades" : [
      { "grade" : 80, "mean" : 75, "std" : 5 },
      { "grade" : 85, "mean" : 100, "std" : 4 },
      { "grade" : 85, "mean" : 100, "std" : 5 }
   ]
}
{
   "_id" : 2,
   "grades" : [
      { "grade" : 90, "mean" : 100, "std" : 5 },
      { "grade" : 87, "mean" : 100, "std" : 3 },
      { "grade" : 85, "mean" : 100, "std" : 4 }
   ]
}

# Update Array Elements Using a Negation Operator
{
   "_id": 1,
   "name": "Christine Franklin",
   "degrees": [
      { "level": "Master",
        "major": "Biology",
        "completion_year": 2010,
        "faculty": "Science"
      },
      {
        "level": "Bachelor",
        "major": "Biology",
        "completion_year": 2008,
        "faculty": "Science"
      }
   ],
   "school_email": "cfranklin@example.edu",
   "email": "christine@example.com"
}
{
   "_id": 2,
   "name": "Reyansh Sengupta",
   "degrees": [
      { "level": "Bachelor",
        "major": "Chemical Engineering",
        "completion_year": 2002,
        "faculty": "Engineering"
      }
   ],
   "school_email": "rsengupta2@example.edu"
}

db.alumni.update({ },{ $set : { "degrees.$[degree].gradcampaign" : 1 } },{ arrayFilters : [ {"degree.level" : { $ne: "Bachelor" } } ],multi : true })

{
   "_id" : 1,
   "name" : "Christine Franklin",
   "degrees" : [
      {
         "level" : "Master",
         "major" : "Biology",
         "completion_year" : 2010,
         "faculty" : "Science",
         "gradcampaign" : 1
      },
      {
         "level" : "Bachelor",
         "major" : "Biology",
         "completion_year" : 2008,
         "faculty" : "Science"
      }
   ],
   "school_email" : "cfranklin@example.edu",
   "email" : "christine@example.com"
}
{
   "_id" : 2,
   "name" : "Reyansh Sengupta",
   "degrees" : [
      {
         "level" : "Bachelor",
         "major" : "Chemical Engineering",
         "completion_year" : 2002,
         "faculty" : "Engineering"
      }
   ],
   "school_email" : "rsengupta2@example.edu"
}

# Update Nested Arrays in Conjunction with $[]
db.students3.insert(
   { "_id" : 1,
      "grades" : [
        { type: "quiz", questions: [ 10, 8, 5 ] },
        { type: "quiz", questions: [ 8, 9, 6 ] },
        { type: "hw", questions: [ 5, 4, 3 ] },
        { type: "exam", questions: [ 25, 10, 23, 0 ] },

      ]
   }
)

db.students3.update({},{ $inc: { "grades.$[t].questions.$[score]": 2 } },{ arrayFilters: [ { "t.type": "quiz" } , { "score": { $gte: 8 } } ], multi: true})

{
   "_id" : 1,
   "grades" : [
      { "type" : "quiz", "questions" : [ 12, 10, 5 ] },
      { "type" : "quiz", "questions" : [ 10, 11, 6 ] },
      { "type" : "hw", "questions" : [ 5, 4, 3 ] },
      { "type" : "exam", "questions" : [ 25, 10, 23, 0 ] }
   ]
}