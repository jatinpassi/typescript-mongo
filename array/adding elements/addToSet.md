## The $addToSet operator adds a value to an array unless the value is already present, in which case $addToSet does nothing to that array.

{ $addToSet: { <field1>: <value1>, ... } }

## $addToSet only ensures that there are no duplicate items added to the set and does not affect existing duplicate elements. $addToSet does not guarantee a particular ordering of elements in the modified set.

## If you use $addToSet on a field that is absent in the document to update, $addToSet creates the array field with the specified value as its element.

# Field is Not an Array -> Failed Operation
{ 
  _id: 1, 
  colors: "blue,green,red" 
}

>> db.foo.update({ _id: 1 },{ $addToSet: { colors: "c" } })
    
    Failed Operation

# Value to Add is An Array

    If the value is an array, $addToSet appends the whole array as a single element.

{ 
  _id: 1,
  letters: ["a", "b"] 
}

>> db.test.update({ _id: 1 },{ $addToSet: { letters: [ "c", "d" ] } })

{ 
  _id: 1, 
  letters: [ "a", "b", [ "c", "d" ] ] 
  }

# Value to Add is a Document

    MongoDB determines that the document is a duplicate if
    1) the existing document has the exact same fields and values
    2) the fields are in the same order. 

    Note: Field order matters and you cannot specify that MongoDB compare only a subset of the fields in the document to determine whether the document is a duplicate of an existing array element.

# $each Modifier
    
    You can use the $addToSet operator with the $each modifier. 
    
    The $each modifier allows the $addToSet operator to add multiple values to the array field.

{ 
  _id: 2, 
  item: "cable", 
  tags: [ "electronics", "supplies" ] 
}

>> db.inventory.update({ _id: 2 },{ $addToSet: { tags: { $each: [ "camera", "electronics", "accessories" ] } } })

{
  _id: 2,
  item: "cable",
  tags: [ "electronics", "supplies", "camera", "accessories" ]
}