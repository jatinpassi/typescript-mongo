## The $min updates the value of the field to a specified value if the specified value is less than the current value of the field. The $min operator can compare values of different types, using the BSON comparison order.

{ $min: { <field1>: <value1>, ... } }

## If the field does not exist, the $min operator sets the field to the specified value.

## For comparisons between values of different types, such as a number and a null, $min uses the BSON comparison order.

{ 
  _id: 1, 
  highScore: 800, 
  lowScore: 200 
}

>> db.scores.update( { _id: 1 }, { $min: { lowScore: 150 } } )

{ 
  _id: 1, 
  highScore: 800, 
  lowScore: 150 
}

>> db.scores.update( { _id: 1 }, { $min: { lowScore: 250 } } )

{ 
  _id: 1, 
  highScore: 800, 
  lowScore: 150 
}

## Use $min to Compare Dates

{
  _id: 1,
  desc: "crafts",
  dateEntered: ISODate("2013-10-01T05:00:00Z"),
  dateExpired: ISODate("2013-10-01T16:38:16Z")
}

>> db.tags.update({ _id: 1 }, { $min: { dateEntered: new Date("2013-09-25") } })

{
  _id: 1,
  desc: "crafts",
  dateEntered: ISODate("2013-09-25T00:00:00Z"),
  dateExpired: ISODate("2013-10-01T16:38:16Z")
}
