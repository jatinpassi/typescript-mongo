## The $sort modifier orders the elements of an array during a $push operation.

##  it must appear with the $each modifier. You can pass an empty array [] to the $each modifier such that only the $sort modifier has an effect.

<!-- @format -->
{
  $push: {
     <field>: {
       $each: [ <value1>, <value2>, ... ],
       $sort: <sort specification>
     }
  }
}
<sort specification> -: 1 = ascending order
                       -1 = descendingorder

## If the array elements are documents, 
## to sort by a field in the documents, specify a sort document with the field and the direction, 
{
  $push: {
     <field>: {
       $each: [ <value1>, <value2>, ... ],
       $sort: { field: <sort specification>}
     }
  }
}
    Note: Do not reference the containing array field in the sort specification (e.g. { "arrayField.field": 1 } is incorrect).

# Sort Array of Documents by a Field in the Documents

{
  "_id": 1,
  "quizzes": [
    { "id" : 1, "score" : 6 },
    { "id" : 2, "score" : 9 }
  ]
}

>> db.students.update({ _id: 1 },{$push: {quizzes: {$each: [ { id: 3, score: 8 }, { id: 4, score: 7 }, { id: 5, score: 6 } ],$sort: { score: 1 }}}})

{
  "_id" : 1,
  "quizzes" : [
     { "id" : 1, "score" : 6 },
     { "id" : 5, "score" : 6 },
     { "id" : 4, "score" : 7 },
     { "id" : 3, "score" : 8 },
     { "id" : 2, "score" : 9 }
  ]
}

# Sort Array Elements That Are Not Documents

{ 
  "_id" : 2, 
  "tests" : [  89,  70,  89,  50 ] 
}

>> db.students.update({ _id: 2 },{ $push: { tests: { $each: [ 40, 60 ], $sort: 1 } } })

{ 
  "_id" : 2, 
  "tests" : [  40,  50,  60,  70,  89,  89 ] 
}

# Update Array Using Sort Only¶
{ 
  "_id" : 3, 
  "tests" : [  89,  70,  100,  20 ] 
}

>> db.students.update({ _id: 3 },{ $push: { tests: { $each: [ ], $sort: -1 } } })

{ 
  "_id" : 3, 
  "tests" : [ 100,  89,  70,  20 ] 
}

# Use $sort with Other $push Modifiers
{
   "_id" : 5,
   "quizzes" : [
      { "wk": 1, "score" : 10 },
      { "wk": 2, "score" : 8 },
      { "wk": 3, "score" : 5 },
      { "wk": 4, "score" : 6 }
   ]
}

>> db.students.update({ _id: 5 },{$push: {quizzes: {$each: [ { wk: 5, score: 8 }, { wk: 6, score: 7 }, { wk: 7, score: 6 } ],$sort: { score: -1 },$slice: 3}}})

{
  "_id" : 5,
  "quizzes" : [
     { "wk" : 1, "score" : 10 },
     { "wk" : 2, "score" : 8 },
     { "wk" : 5, "score" : 8 }
  ]
}