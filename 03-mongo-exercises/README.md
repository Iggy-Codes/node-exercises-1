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
```

### Propagate your filters

> Prepare your filters so they can be applied in ALL your endpoints

`http://localhost:3000/restaurants/borough/Brooklyn?hide=_id&show=name`

should return...
```
[
  {name: "Riviera Caterer"},
  {name: "Wendy'S"},
  {name: "Wilken'S Fine Food"},
  {name: "Taste The Tropics Ice Cream"},
  ...
]
```

### _GET_ `/restaurants?limit=5`

Limit the number of results using another param in the url

`http://localhost:3000/restaurants/borough/Brooklyn?hide=_id&show=name&limit=5`

```
[
  { name: "Riviera Caterer" },
  { name: "Wendy'S" },
  { name: "Wilken'S Fine Food" },
  { name: "Taste The Tropics Ice Cream" },
  { name: "Regina Caterers" }
]
```

> Hint: Use [Cursor method](https://mongodb.github.io/node-mongodb-native/api-generated/cursor.html) `limit()`

### _GET_ `/restaurants?limit=5&page=2`

Add a paging parameter based on the limit parameter

`http://localhost:3000/restaurants?hide=_id&show=name&limit=5&page=2`

```
[
  { name: "Wendy'S" },
  { name: "Dj Reynolds Pub And Restaurant" },
  { name: "Tov Kosher Kitchen" },
  { name: "Brunos On The Boulevard" },
  { name: "Kosher Island" }
]
```

> Hint: Use [Cursor method](https://mongodb.github.io/node-mongodb-native/api-generated/cursor.html) `skip()`

### _GET_ `/restaurants/cuisine/:cuisine` & `/restaurants/cuisine/not/:cuisine`

Get the restaurants with a cuusine equal and not equal some value

> Hint: Use the same function to handle both endpoins: w/ and wo/ `/not/`

`http://localhost:3000/restaurants/cuisine/Bakery?hide=_id&show=name,cuisine&limit=3`

```
[
  { cuisine: "Bakery", name: "Morris Park Bake Shop" },
  { cuisine: "Bakery", name: "Olive'S" },
  { cuisine: "Bakery", name: "Tropical House Baking Co." }
]
```

`http://localhost:3000/restaurants/cuisine/not/Bakery?hide=_id&show=name,cuisine&limit=3`

```
[
  { cuisine: "American ", name: "Riviera Caterer" },
  { cuisine: "Hamburgers", name: "Wendy'S" },
  { cuisine: "Irish", name: "Dj Reynolds Pub And Restaurant" }
]
```


### _GET_ `/restaurant/:id`

Get details of a restaurant based on its ID

### _GET_ `/restaurant/:id/around/:km` ** **_Super challenge!!!_**

Get restaurants that are located a specific number of kilometers around of another one

`http://localhost:3000/restaurant/57ab1efcfd999cac66e8ef89/around/5?show=address.coord,name&hide=_id&limit=5`

should return restaurants 5km around the one selected...

```
[
  {
    address: {
      coord: [ -73.98241999999999, 40.579505 ]
    },
    name: "Riviera Caterer"
  },
  {
    address: {
      coord: [ -73.983564, 40.579355 ]
    },
    name: "Pop'S Restaurant"
  },
  {
    address: {
      coord: [ -73.9838125, 40.5788295 ]
    },
    name: "Totonno'S Pizzeria"
  },
  {
    address: {
      coord: [ -73.9839533, 40.578876 ]
    },
    name: "Primorskiy Corp."
  },
  {
    address: {
      coord: [ -73.9805707, 40.5793238 ]
    },
    name: "Domino'S Pizza"
  }
]
```


> Hint: You will need to index using a 2dsphere to this kind of queries. Go to the shell and do...

```
  db.restaurants.ensureIndex({ "address.coord":"2dsphere"}); 
```

> After this you'll be able to do queries like this

```
  db.collection("restaurants").find( {
    "address.coord" : {
         $near: {
             $geometry: {
                type: "Point" ,
                coordinates: [ longitude , latitude ]
             },
             $maxDistance: km*1000
          }
        }
      }
    )
```

More info: 
- [`$near`](https://docs.mongodb.com/manual/reference/operator/query/near/#op._S_near)
- https://docs.mongodb.com/manual/tutorial/geospatial-tutorial/
- https://myadventuresincoding.wordpress.com/2011/10/02/mongodb-geospatial-queries/
- http://dataops.co/working-with-geospatial-support-in-mongodb/


