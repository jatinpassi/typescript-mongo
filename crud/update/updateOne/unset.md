{
  "_id" : ObjectId("4b253b067525f35f94b60a31"),
  "name" : "joe",
  "age" : 30,
  "sex" : "male",
  "location" : "Wisconsin",
  "favorite book" : ["Cat's Cradle", "Foundation Trilogy", "Ender's Game"]
}

## If the user realizes that he actually doesn’t like reading, he can remove the key altogether with "$unset":

>> db.users.updateOne({"name" : "joe"}, {"$unset" : {"favorite book" : 1}})

{
  "_id" : ObjectId("4b253b067525f35f94b60a31"),
  "name" : "joe",
  "age" : 30,
  "sex" : "male",
  "location" : "Wisconsin"
}