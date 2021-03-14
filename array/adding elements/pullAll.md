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

# order matters - 1
{
    user:user_name, 
    streams:[
        {user:user_a, name:name_a}, 
        {user:user_b, name:name_b},
        {user:user_c, name:name_c}
    ]
}

>> db.streams.update({name:"user_name", {"$pullAll:{streams:[{user:"user_a", name:"name_a"},{user:"user_b", name:"name_b"}]}})

## This will work {user:"user_a", name:"name_a"}and {user:"user_b", name:"name_b"} match the order

# order matters - 2
{
    user:user_name, 
    streams:[
        {user:user_a, name:name_a}, 
        {user:user_b, name:name_b},
        {user:user_c, name:name_c}
    ]
}

>> db.streams.update({name:"user_name", {"$pullAll:{streams:[{name:"name_a", user:"user_a"},{name:"name_b", user:"user_b"}]}})

## This will not work because none {name:"name_a", user:"user_a"},{name:"name_b", user:"user_b"} matches the order.