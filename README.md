Run this container, assuming you have docker-compose installed:
```bash 
docker-compose up -d
```

Start and stop the container
```bash 
docker-compose start
docker-compose stop
```

Test the connection with `mongosh`

```bash 
mongosh -u fixme -p fixme 

test> show databases
```

Some examples:
```mongodb 
test> use animals
animals> db.animals.insertMany([
...     { name: "Lion", species: "Panthera leo" },
...     { name: "Tiger", species: "Panthera tigris" },
...     { name: "Elephant", species: "Loxodonta africana" },
...     { name: "Giraffe", species: "Giraffa camelopardalis" },
...     { name: "Monkey", species: "Cercopithecidae" }
...     // Add more animal names and species as needed
... ])
animals> db.animals.find({ name: "Lion" })
[
  {
    _id: ObjectId("659d78afeeeae1ea378ef21b"),
    name: 'Lion',
    species: 'Panthera leo'
  }
]
```

---


Connect to the mongoDB database inside the app

```bash
npm install mongodb
```

Indide server.js:
```javascript 
const {MongoClient} = require("mongodb");
let db

// ------
// -----

async function start() {
  const client = new MongoClient("mongodb://fixme:fixme@localhost:27017/animals?&authSource=admin");
  // Let's wait for the connection
  await client.connect()
  // This return the database
  db = client.db()
  // We are connecting to the database before we start the connection
  app.listen(3001)
}

start()
```
Let's make a query

```javascript
app.get("/", async (req, res) => {

  // A query
  // We want to await the query because we don't know how long it will take
  const allAnimals = await db.collection("animals").find().toArray();
  console.log(allAnimald)
  
  res.send("Welcome to the homepage")
})
```

Now when we'll connect to the website, the output of the query will be displayed in the terminal
