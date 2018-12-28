---
layout: post
title: "Building a REST API with mongoose" 
author: "Lasse Schultebraucks"
categories:  JavaScript
thumbnail: "/assets/mongodb.png"
---

Mongoose is a node module and works as an API for MongoDB, which is a NoSQL database. With the power of mongoose you can easily write schemas for your MongoDB database and perform CRUD operations on it.

To use mongoose, Node.JS and MongoDB is required. You can get NodeJS [here](https://nodejs.org/en/) and MongoDB over [here](https://docs.mongodb.com/manual/installation/).

After creating a new npm project with `npm init` we can install ExpressJS, which we need for running our backend server, body-parser to parse the body of incoming requests and mongoose, an API to access the MongoDB, with following command:

```
npm install --save express body-parser mongoose
```

We can create our express server in just a couple of lines.

```javascript
"use strict"

const express = require('express');
const app = express();

const port = 8080;

app.get('/', (req,res) => {
    res.send("api works");
});

app.listen(port, () => {
    console.log('app listening on port', port);
});
```

Now if we run `node app.js` and go to http://localhost:8080 in our browser, we should see the text `api works` which means that our express server runs. 

Next we implement a simple database schema for our REST API. It will represent a simple model of a car with name, manufacturer and price.

```javascript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const CarSchema = new Schema({
    name: String,
    manufacturer: String,
    price: Number
});

module.exports = mongoose.model('Car', CarSchema);
```

In the first line we imported mongoose. Then we created a schema for our NoSQL database. There are many more options how you can define your schema, which you can check out over [here](http://mongoosejs.com/docs/guide.html). Last but not least we exported our schema to use it in the app.js, which already runs our express server.

Now we can implement the CRUD operations with mongoose for our MongoDB database.

```javascript
"use strict"

const express = require('express');
const app = express();
const bodyParser = require('body-parser')
const mongoose = require('mongoose')
const Car = require('./car.model');  

const db = 'mongodb://localhost/example';
const port = 8080;

mongoose.connect(db);

app.use(bodyParser.json())
app.use(bodyParser.urlencoded({
    extended: true
}));

app.get('/', (req,res) => {
    res.send("api works");
});


// get all cars
app.get('/cars', (req,res) => {
    console.log('get all cars');
    Car.find({}).exec((err,cars) => {
        if(err) {
            res.send('error has occured in /cars')
        } else {
            console.log(cars);
            res.json(cars);
        }
    });
});

// get car by id
app.get('/car/:id', (req,res) => {
    console.log('get', req.params.id, 'car');
    Car.findOne({
        _id: req.params.id
    }).exec((err,car) => {
        if (err) {
            res.send('error has occured in /car/:id');
        } else {
            console.log(car);
            res.json(car);
        }
    });
});

// post car
app.post('/car', (req,res) => {
    var car = new Car();

    car.name = req.body.name;
    car.manufacturer = req.body.manufacturer;
    car.price = req.body.price;

    car.save((err,car) => {
        if (err) {
            res.send('error saving car');
        } else {
            console.log('save car', car);
            res.send(car);
        }
    });
});

// alternative post car
app.post('/car2', (req,res) => {
    Car.create(req.body, (err,car) => {
        if (err) {
            res.send('error saving car');
        } else {
            console.log('save car', car);
            res.send(car);
        }
    })
});

// update model name of existing car
app.put('/car/:id', (req,res) => {
    Car.findOneAndUpdate({
        _id: req.params.id
    }, 
    {$set: 
        {name: req.body.name}},
         {upsert: true},
          (err,car) => {
             if (err) {
                res.send('error occured while trying to update car')
             } else {
                 console.log('updated car', car);
                 res.send(200);
             }
    });
});

// delete car
app.delete('/car/:id', (req,res) => {
    Car.findOneAndRemove({
        _id: req.params.id
    }, (err,car)  => {
        if (err) {
            res.send('error occured while trying to delete car');
        } else {
            console.log(car, 'deleted');
            res.send(200);
        }
    });
});

app.listen(port, () => {
    console.log('app listening on port', port);
});
```

Before rerunning our server, we have to be sure that we run an instance of `mongod` in another shell window. If that is the case, we can rerun our express server. To test the complete features from the build REST API I recommend you [Postman](https://www.getpostman.com/) or another similar API development environment.

Mongoose offers much more features, so be sure to check out their official documentations if you want dive deeper into the topic.

All written code is also available on [GitHub](https://github.com/LSchultebraucks/mongoose-crud-example).