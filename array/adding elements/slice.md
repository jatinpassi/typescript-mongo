## The $slice modifier limits the number of array elements during a $push operation.

## To use the $slice modifier, it must appear with the $each modifier. You can pass an empty array [] to the $each modifier such that only the $slice modifier has an effect.
<!-- @format -->
{
  $push: {
     <field>: {
       $each: [ <value1>, <value2>, ... ],
       $slice: <num>
     }
  }
}

<num> -: Zero =	To update the array <field> to an empty array.
      -: Negative = To update the array <field> to contain only the last <num> elements.
      -: Positive = To update the array <field> contain only the first <num> elements.

# Slice from the End of the Array
{ 
  "_id" : 1, 
  "scores" : [ 40, 50, 60 ] 
}

>> db.students.update({ _id: 1 },{$push: {scores: {$each: [ 80, 78, 86 ],$slice: -5}}})

{ 
  "_id" : 1, 
  "scores" : [  50,  60,  80,  78,  86 ] 
}

# Slice from the Front of the Array
{ 
  "_id" : 2, 
  "scores" : [ 89, 90 ] 
}

>> db.students.update({ _id: 2 },{$push: {scores: {$each: [ 100, 20 ],$slice: 3}}})

{ 
  "_id" : 2, 
  "scores" : [  89,  90,  100 ] 
}

# Update Array Using Slice Only

{ 
  "_id" : 3, 
  "scores" : [  89,  70,  100,  20 ] 
}

>> db.students.update({ _id: 3 },{$push: {scores: {$each: [ ],$slice: -3}})

{ 
  "_id" : 3, 
  "scores" : [  70,  100,  20 ] 
}

# Use $slice with Other $push Modifiers

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