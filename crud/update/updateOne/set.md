## "$set" sets the value of a field. If the field does not yet exist, it will be created.
    
    db.users.updateOne({"_id" : ObjectId("4b253b067525f35f94b60a31")}, {"$set" : {"favorite book" : "War and Peace"}})

> db.users.findOne({"_id" : ObjectId("4b253b067525f35f94b60a31")})
> {
   "_id" : ObjectId("4b253b067525f35f94b60a31"),
   "name" : "joe",
   "age" : 30,
   "sex" : "male",
   "location" : "Wisconsin",
   "favorite book" : "War and Peace"
  }