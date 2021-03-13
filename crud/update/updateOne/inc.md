## Every time someone visits a page, we can find the page by its URL and use the "$inc" modifier to increment the value of the "pageviews" key:

{
  "_id" : ObjectId("4b253b067525f35f94b60a31"),
  "url" : "www.example.com",
  "pageviews" : 52
}

>> db.analytics.updateOne({"url" : "www.example.com"}, {"$inc" : {"pageviews" : 1}})


//output: { "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }


{
  "_id" : ObjectId("4b253b067525f35f94b60a31"),
  "url" : "www.example.com",
  "pageviews" : 53
}

    ---------------------------OR----------------------------

## y, let’s say that the base unit of points a player can earn is 50. We can use the "$inc" modifier to add 50 to the player’s score

{
  "_id" : ObjectId("4b2d75476cc613d5ee930164"),
  "game" : "pinball",
  "user" : "joe",
}

>> db.games.updateOne({"game" : "pinball", "user" : "joe"}, {"$inc" : {"score" : 50}})

{ 
  "_id" : ObjectId("4b2d75476cc613d5ee930164"),
  "game" : "pinball",
  "user" : "joe",
  "score" : 50
}

>> db.games.updateOne({"game" : "pinball", "user" : "joe"}, {"$inc" : {"score" : 10000}})

{
  "_id" : ObjectId("4b2d75476cc613d5ee930164"),
  "game" : "pinball",
  "user" : "joe",
  "score" : 10050
}