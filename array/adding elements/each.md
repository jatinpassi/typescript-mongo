## The $each modifier is available for use with the $addToSet operator and the $push operator.

## Use with the $addToSet operator to add multiple values to an array <field> if the values do not exist in the <field>.

{ $push: { <field>: { $each: [ <value1>, <value2> ... ] } } }


{
  _id: 2,
  item: "cable",
}

>> db.inventory.update({ _id: 2 },{ $push: { tags: { $each: [ "camera", "electronics", "accessories" ] } } } )

{
  _id: 2,
  item: "cable",
  tags: [ "camera", "electronics", "accessories" ]
}


>> db.inventory.update({ _id: 2 },{ $push: { tags: { $each: [ "supplies" ] } } } )


{
  _id: 2,
  item: "cable",
  tags: [ "camera", "electronics", "accessories","supplies"]
}