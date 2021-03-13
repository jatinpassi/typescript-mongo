## The $currentDate operator sets the value of a field to the current date, either as a Date or a timestamp.

## The default type is Date.

{ $currentDate: { <field1>: <typeSpecification1>, ... } }

<typeSpecification> can be either:
    1) a boolean true to set the field value to the current date as a Date
    2) a document { $type: "timestamp" } or { $type: "date" } which explicitly specifies the type. The operator is case-sensitive and accepts only the lowercase "timestamp" or the lowercase "date".

# If the field does not exist, $currentDate adds the field to a document.

>> db.customers.insertOne(
      { _id: 1, status: "a", lastModified: ISODate("2013-10-02T01:11:18.965Z") }
   )

{ 
  _id: 1, 
  status: "a", 
  lastModified: ISODate("2013-10-02T01:11:18.965Z") 
}

>> db.customers.updateOne(
    { _id: 1 },
    {
      $currentDate: {
        lastModified: true,
        "cancellation.date": { $type: "timestamp" }
      },
      $set: {
       "cancellation.reason": "user request",
        status: "D"
    }
   }
   )

{
   "_id" : 1,
   "status" : "D",
   "lastModified" : ISODate("2020-01-22T21:21:41.052Z"),
   "cancellation" : {
      "date" : Timestamp(1579728101, 1),
      "reason" : "user request"
   }
}