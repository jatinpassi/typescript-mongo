## Unlike the $pull operator that removes elements by specifying a query, $pullAll removes elements that match the listed values.

{ $pullAll: { <field1>: [ <value1>, <value2> ... ], ... } }

## If a <value> to remove is a document or an array, $pullAll removes only the elements in the array that match the specified <value> exactly, including order.

{ 
  _id: 1, 
  scores: [ 0, 2, 5, 5, 1, 0 ] 
}

>> db.survey.update( { _id: 1 }, { $pullAll: { scores: [ 0, 5 ] } } )

{ 
  "_id" : 1, 
  "scores" : [ 2, 1 ] 
}
