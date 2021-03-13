## Multiplies the value of the field by the specified amount.

{ 
  "_id" : 1,
  "item" : "ABC", 
  "price" : NumberDecimal("10.99"), 
  "qty" : 25 
}

##  using the $mul operator to multiply the price by 1.25 and the qty field by 2.
>> db.products.update({ _id: 1 },{ $mul: { price: NumberDecimal("1.25"), qty: 2 } })


{ 
  "_id" : 1, 
  "item" : "ABC", 
  "price" : NumberDecimal("13.7375"), 
  "qty" : 50 
}
