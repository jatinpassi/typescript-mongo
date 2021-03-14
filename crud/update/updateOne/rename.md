## The $rename operator updates the name of a field

{$rename: { <field1OldName>: <field1NewName>, ... } }

{
  "_id": 1,
  "alias": [ "The American Cincinnatus", "The American Fabius" ],
  "cell": "555-555-5555",
  "nmae": { "first" : "george", "last" : "washington" }
}

>> db.students.updateMany( {}, { $rename: { "nmae": "name",'cell': 'mobile' } } )

{
  "_id": 1,
  "alias": [ "The American Cincinnatus", "The American Fabius" ],
  "mobile": "555-555-5555",
  "name": { "first" : "george", "last" : "washington" }
}

## Rename a Field in an Embedded Document

{
  "_id": 1,
  "alias": [ "The American Cincinnatus", "The American Fabius" ],
  "mobile": "555-555-5555",
  "name": { "first" : "george", "last" : "washington" }
}

>> db.students.update( { _id: 1 }, { $rename: { "name.first": "name.fname" } } )

{
  "_id" : 1,
  "alias" : [ "The American Cincinnatus", "The American Fabius" ],
  "mobile" : "555-555-5555",
  "name" : { "fname" : "george", "last" : "washington" }
}

    Note: When renaming a field and the existing field name refers to a field that does not exist, the $rename operator does nothing