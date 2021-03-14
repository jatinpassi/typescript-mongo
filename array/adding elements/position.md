## The $position modifier specifies the location in the array at which the $push operator inserts elements.
## Without the $position modifier, the $push operator inserts elements to the end of the array.

<!-- @format -->

{
  $push: {
    <field>: {
       $each: [ <value1>, <value2>, ... ],
       $position: <num>
    }
  }
}

<num> indicates the position in the array
    A positive number corresponds to the position in the array, starting from the beginning of the array. If the value of <num> is greater or equal to the length of the array, the $position modifier has no effect and $push adds elements to the end of the array.

    A negative number corresponds to the position in the array, counting from (but not including) the last element of the array. For example, -1 indicates the position just before the last element in the array.


# Add Elements at the Start of the Array
{ 
  "_id" : 1, 
  "scores" : [ 100 ] 
}

>> db.students.update({ _id: 1 },{$push: {scores: {$each: [ 50, 60, 70 ],$position: 0}}}
)

{ 
  "_id" : 1, 
  "scores" : [  50,  60,  70,  100 ] 
}

# Add Elements to the Middle of the Array

{ 
  "_id" : 1, 
  "scores" : [  50,  60,  70,  100 ] 
}

>> db.students.update({ _id: 1 },{$push: {scores: {$each: [ 20, 30 ],$position: 2}}})

{ 
  "_id" : 1, 
  "scores" : [  50,  60,  20,  30,  70,  100 ] 
}

# Use a Negative Index to Add Elements to the array

{ 
  "_id" : 1, 
  "scores" : [  50,  60,  20,  30,  70,  100 ] 
}

>> db.students.update({ _id: 1 },{$push: {scores: {$each: [ 90, 80 ],$position: -2}}})

{ 
  "_id" : 1, 
  "scores" : [ 50, 60, 20, 30, 90, 80, 70, 100 ] 
}

