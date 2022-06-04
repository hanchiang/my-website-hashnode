## Managing connections in MongoDB

## Introduction to MongoDB

MongoDB is a NoSQL database that allows us to store data in other formats such as object, document, graph other than tabular relations using relational databases. As it is designed for horizontal scaling, NoSQL is increasingly used in applications that handle huge volume of data, for use cases such as analytics.

Personally, I am using MongoDB out of interest, to learn a new technology and apply it to in my own side project. Like others, I followed tutorials and courses which used mongoose, a Object Document Mapper for MongoDB, to abstract nitty-gritty details away to provide a streamlined learning experience to new developers.

However, this time, I am using the [native driver](https://mongodb.github.io/node-mongodb-native/), in an attempt to learn to use the actual MongoDB syntax and commands.  
So, the very first roadblock that I faced is to manage the connections, which mongoose already takes good care of.

## Managing connections

A quick research on StackOverflow and other websites suggest the following:

1.  Connect to MongoDB when the app starts
2.  Reuse the connection whenever the route handlers need to connect to the database

Sounds pretty simple. The next question I had in mind was: What is a good way to do this?

Another quick search showed a few approaches:

1.  Connect to and store the MongoDB instance in **req** object via an application level middleware before hitting the route handlers
2.  Connect to MongoDB and pass the instance to the route handlers, which does so by exporting a function
3.  Connect to MongoDB and store the instance. Route handlers and controllers that need access to the database will retrieve the instance

### Approach 1: Storing DB instance in request object

This approach is the easiest to implement.  
Set up a middleware that connects to the database, store the database instance in the request object, and use it in the route handlers.

[express-mongo-db](https://github.com/floatdrop/express-mongo-db/blob/master/index.js) provides an example of how to do this.

However, I found out via StackOverflow that it might not be a good idea to expose library codes in an object.

### Approach 2: Pass DB instance to route handlers

This approach is detailed by [mLab](https://blog.mlab.com/2017/05/mongodb-connection-pooling-for-express-applications/), one of the most popular MongoDB cloud providers.

1. Setup up the connections in a config file and export the database instances.

```javascript
// Note: A production application should not expose database credentials in plain text.
// For strategies on handling credentials, visit 12factor: https://12factor.net/config.
const MongoClient = require('mongodb').MongoClient;
const PROD_URI = "mongodb://:@:,:/?replicaSet="
const MKTG_URI = "mongodb://:@:,:/?replicaSet="
function connect(url) {
  return MongoClient.connect(url).then(client => client.db())
}
module.exports = async function() {
  let databases = await Promise.all([connect(PROD_URI), connect(MKTG_URI)])
  return { production: databases[0], marketing: databases[1] }
}
```

2. Pass **app** and **dbs** to route handlers to start working with the database.

```javascript
module.exports = function(app, dbs) {
  app.get('/production', (req, res) => {
    dbs.production.collection('test').find({}).toArray((err, docs) => {
      if (err) {
        console.log(err) res.error(err)
      } else {
         res.json(docs)
      }
   })
  })
  app.get('/marketing', (req, res) => {
    dbs.marketing.collection('test').find({}).toArray((err, docs) => {
      if (err) {
        console.log(err) res.error(err)
      } else {
        res.json(docs)
      }
    })
  })
  return app
}
```

3. Wire up the database connection, app and route handlers

```javascript
const express = require('express')
const app = express()
const initializeDatabases = require('./dbs')
const routes = require('./routes')

initializeDatabases().then(dbs => {
  // Initialize the application once database connections are ready.
  routes(app, dbs)
    .listen(3000, () => console.log('Listening on port 3000')) })
    .catch(err => {
        console.error('Failed to make all database connections!')
        console.error(err)
        process.exit(1)
})
```

Even though this approach works, it won't be as clean for my case as my route controllers are defined in its own file, not in the route handler itself. Therefore, one additional hoop is needed to pass the database instance.

### Approach 3: Store DB instance in a file

I decided to search around for a bit more and found another method which I like best.

1. Set up a `connect()` and `get()` function in the database config file

```javascript
const MongoClient = require('mongodb').MongoClient;

let db;
exports.connect = () => {
  if (!db) {
    return MongoClient.connect(process.env.DATABASE, { useNewUrlParser: true })             
      .then(connection => {
         db = connection.db();
         console.log('Connected to database 👍👍👍'); 
       })
       .catch(err => {
          db = undefined;
          console.log(`☹☹☹ ${err.message}`);
        });
  } 
  return Promise.resolve()
}
exports.get = () => db;    
```

2. Connect to database and start express

```javascript
const db = require('./db');
const app = require('./app');
const port = process.env.PORT || 7777;

db.connect()
  .then(() => {
     const server = app.listen(port, () => {
       console.log(`Express running → PORT ${port}`);
     });
 });
```

3. Require the database instance in route handlers/controllers

```javascript
const db = require('../db');
exports.getStores = async (req, res) => {
  const stores = await db.get().collection('stores').find({}).toArray();
  res.render('stores', { title: 'Stores', stores });
}
```

## Closing

These approaches achieve a common goal: To create a single connection instance that allows it to be reused. The third approach does this in the simplest way that follows proper coding practices.