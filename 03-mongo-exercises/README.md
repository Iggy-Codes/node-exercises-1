# MongoDB - JS exercises

## Restaurant Collection 

Import the collection `restaurant` following the instructions here: https://docs.mongodb.com/getting-started/shell/import-data/

```
{
  "address": {
     "building": "1007",
     "coord": [ -73.856077, 40.848447 ],
     "street": "Morris Park Ave",
     "zipcode": "10462"
  },
  "borough": "Bronx",
  "cuisine": "Bakery",
  "grades": [
     { "date": { "$date": 1393804800000 }, "grade": "A", "score": 2 },
     { "date": { "$date": 1378857600000 }, "grade": "A", "score": 6 },
     { "date": { "$date": 1358985600000 }, "grade": "A", "score": 10 },
     { "date": { "$date": 1322006400000 }, "grade": "A", "score": 9 },
     { "date": { "$date": 1299715200000 }, "grade": "B", "score": 14 }
  ],
  "name": "Morris Park Bake Shop",
  "restaurant_id": "30075445"
}
```

---

Create an express app that returns a JSON containing the following data from this collection stored in MongoDB

### _GET_ `/restaurants`

All restaurants in the collection

### _GET_ `/restaurants?show=_id,name,borough,cuisine` 

Show only the fields specified in the query `fields` for all the documents in the collection restaurant. 

### _GET_ `/restaurants?hide=_id` 

Hide the fields specified in the query `fieldshide` for all the documents in the collection restaurant. 

Both queries could be used at the same time

`http://localhost:3000/restaurants?hide=_id&show=name,cuisine`

should return...

```
  [
    {"cuisine":"Bakery","name":"Morris Park Bake Shop"},
    {"cuisine":"American ","name":"Riviera Caterer"},
    {"cuisine":"Hamburgers","name":"Wendy'S"},
    ....
  ]
```

### _GET_ `/restaurants/borough/:borough`

Return all the restaurants filtered by borough (for example the ones in the borough `Bronx`.)

`http://localhost:3000/restaurants/borough/Bronx`

should return...

```
[  
   {  
      "_id":"57ab1efcfd999cac66e8ef88",
      "address":{  
         "building":"1007",
         "coord":[  
            -73.856077,
            40.848447
         ],
         "street":"Morris Park Ave",
         "zipcode":"10462"
      },
      "borough":"Bronx",
      "cuisine":"Bakery",
      "grades":[...],
      "name":"Morris Park Bake Shop",
      "restaurant_id":"30075445"
   },
   {  
      "_id":"57ab1efcfd999cac66e8ef92",
      "address":{  
         "building":"2300",
         "coord":[  
            -73.8786113,
            40.8502883
         ],
         "street":"Southern Boulevard",
         "zipcode":"10460"
      },
      "borough":"Bronx",
      "cuisine":"American ",
      "grades":[...],
      "name":"Wild Asia",
      "restaurant_id":"40357217"
   },
   ...
  ]
``


