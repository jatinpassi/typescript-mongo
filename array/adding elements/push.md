## "$push" adds elements to the end of an array if the array exists and creates a new array if it does not. 

{
  "_id" : ObjectId("4b2d75476cc613d5ee930164"),
  "title" : "A blog post",
  "content" : "...",
}


>> > db.blog.posts.updateOne({"title" : "A blog post"}, {"$push" : {"comments" : {"name" : "joe", "email" : "joe@example.com", "content" : "nice post."}}})
// ouput : { "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }


{
  "_id" : ObjectId("4b2d75476cc613d5ee930164"),
  "title" : "A blog post",
  "content" : "...",
  "comments" : [
    {
      "name" : "joe",
      "email" : "joe@example.com",
      "content" : "nice post."
    }
  ]
}


>> db.blog.posts.updateOne({"title" : "A blog post"}, {"$push" : {"comments" : {"name" : "bob", "email" : "bob@example.com", "content" : "good post."}}})
// output : { "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
{
  "_id" : ObjectId("4b2d75476cc613d5ee930164"),
  "title" : "A blog post",
  "content" : "...",
  "comments" : [
    {
      "name" : "joe",
      "email" : "joe@example.com",
      "content" : "nice post."
    },
    {
      "name" : "bob",
      "email" : "bob@example.com",
      "content" : "good post."
    }
  ]
}