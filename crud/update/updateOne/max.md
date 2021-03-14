## The $max operator updates the value of the field to a specified value if the specified value is greater than the current value of the field. The $max operator can compare values of different types, using the BSON comparison order.

{ $max: { <field1>: <value1>, ... } }

{ 
  _id: 1, 
  highScore: 800, 
  lowScore: 200 
}

>> db.scores.update( { _id: 1 }, { $max: { highScore: 950 } } )

{ 
  _id: 1, 
  highScore: 950, 
  lowScore: 200 
}

>> db.scores.update( { _id: 1 }, { $max: { highScore: 870 } } )

{ 
  _id: 1, 
  highScore: 950, 
  lowScore: 200 
}

## Use $max to Compare Dates¶

{
  _id: 1,
  desc: "crafts",
  dateEntered: ISODate("2013-10-01T05:00:00Z"),
  dateExpired: ISODate("2013-10-01T16:38:16.163Z")
}

>> db.tags.update({ _id: 1 },{ $max: { dateExpired: new Date("2013-09-30") } })

{
   _id: 1,
   desc: "decorative arts",
   dateEntered: ISODate("2013-10-01T05:00:00Z"),
   dateExpired: ISODate("2013-10-01T16:38:16.163Z")
}