## The $pull operator removes from an existing array all instances of a value or values that match a specified condition.

{ $pull: { <field1>: <value|condition>, <field2>: <value|condition>, ... } }

## If you specify a <condition> and the array elements are embedded documents, $pull operator applies the <condition> as if each array element were a document in a collection.

## If the specified <value> to remove is an array, $pull removes only the elements in the array that match the specified <value> exactly, including order.

## If the specified <value> to remove is a document, $pull removes only the elements in the array that have the exact same fields and values. The ordering of the fields can differ.

# Remove All Items That Equal a Specified Value

{
   _id: 1,
   fruits: [ "apples", "pears", "oranges", "grapes", "bananas" ],
   vegetables: [ "carrots", "celery", "squash", "carrots" ]
}
{
   _id: 2,
   fruits: [ "plums", "kiwis", "oranges", "bananas", "apples" ],
   vegetables: [ "broccoli", "zucchini", "carrots", "onions" ]
}

>> db.stores.update({ },{ $pull: { fruits: { $in: [ "apples", "oranges" ] }, vegetables: "carrots" } },{ multi: true })

{
  "_id" : 1,
  "fruits" : [ "pears", "grapes", "bananas" ],
  "vegetables" : [ "celery", "squash" ]
}
{
  "_id" : 2,
  "fruits" : [ "plums", "kiwis", "bananas" ],
  "vegetables" : [ "broccoli", "zucchini", "onions" ]
}

# Remove All Items That Match a Specified $pull Condition¶
{ 
  _id: 1, 
  votes: [ 3, 5, 6, 7, 7, 8 ] 
}

>> db.profiles.update( { _id: 1 }, { $pull: { votes: { $gte: 6 } } } )

{ 
  _id: 1, 
  votes: [  3,  5 ] 
}

# Remove Items from an Array of Documents
{
   _id: 1,
   results: [
      { item: "A", score: 5 },
      { item: "B", score: 8, comment: "Strongly agree" }
   ]
}
{
   _id: 2,
   results: [
      { item: "C", score: 8, comment: "Strongly agree" },
      { item: "B", score: 4 }
   ]
}

>> db.survey.update({ },{ $pull: { results: { score: 8 , item: "B" } } },{ multi: true })

    -----------------------------OR--------------------------

>> db.survey.update({ },{ $pull: { results: { $elemMatch: { score: 8 , item: "B" } } } },{ multi: true })

{
   "_id" : 1,
   "results" : [ { "item" : "A", "score" : 5 } ]
}
{
  "_id" : 2,
  "results" : [
      { "item" : "C", "score" : 8, "comment" : "Strongly agree" },
      { "item" : "B", "score" : 4 }
   ]
}