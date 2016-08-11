# MongoDB - JS exercises

## Restaurant Collection 

Use your DB test to insert this data in the `restaurants` collection

```
db.restaurants.insertOne({
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
})
```

---

Create an express app that returns a JSON containing the following data from this collection stored in MongoDB

### `/restaurants` _GET_

Write a MongoDB query to display all the documents in the collection restaurants.

### `/restaurants?fields=_id, name, borough, cuisine` _GET_

Write a MongoDB query to display the fields restaurant_id, name, borough and cuisine for all the documents in the collection restaurant. 

