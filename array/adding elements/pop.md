## The $pop operator removes the first or last element of an array.

{ $pop: { <field>: <-1 | 1>, ... } }
 
$pop a value of -1 to remove the first element of an array.
                 1 to remove the last element in an array.

## The $pop operation fails if the <field> is not an array.
## If the $pop operator removes the last item in the <field>, the <field> will then hold an empty array.

# Remove the First Item of an Array
{ 
  _id: 1, 
  scores: [ 8, 9, 10 ]
}

>> db.students.update( { _id: 1 }, { $pop: { scores: -1 } } )

{ 
  _id: 1, 
  scores: [ 9, 10 ] 
}

# Remove the Last Item of an Array
{ 
  _id: 1, 
  scores: [ 9, 10 ] 
}

>> db.students.update( { _id: 1 }, { $pop: { scores: 1 } } )

{ 
  _id: 1, 
  scores: [ 9 ] 
}
