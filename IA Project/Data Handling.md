	Now schemas in mongo plays a important role for validation.   
when using a database with Express applications, models and schemas are concepts often associated with Object-Relational Mapping (ORM) libraries like Mongoose.

## Schema & Model:

- **Definition:** A schema defines the structure and constraints of the data stored in a database. It specifies the fields, their types, and any additional rules such as required fields or default values. ^e17f98
- **In Express/Mongoose:** When using Mongoose (a popular MongoDB ODM for Node.js), you define a schema to represent the structure of your documents in a MongoDB collection.
- **Example:**
  
```js
const mongoose = require('mongoose');  const userSchema = new mongoose.Schema({   firstName: 
		{ type: String,
		  required: true },
lastName: 
		{ type: String,
		  required: true },
email: 
		{ type: String,
		  required: true,
		  unique: true },
age: 
		  { type: Number },
		   });
module.exports = mongoose.model('User', userSchema);
//by default, MongoDB automatically pluralizes collection names when creating them based on the names of Mongoose models. This behavior is part of the Mongoose model naming conventions.We can avoid this by andding our collection name at last once again.
module.exports = mongoose.model('User', userSchema ,'User');
```

- **Definition:** A model is a higher-level abstraction that represents a specific type of data in your application. It typically interacts with the underlying database, providing an interface for querying and manipulating the data.
- **In Express/Mongoose:** In the context of Mongoose, a model is a constructor function that represents a collection in MongoDB. It provides methods for querying and updating documents in that collection.
- **Example:**
    
```js   
const mongoose = require('mongoose'); 
const userSchema = require('./userSchema');
const User = mongoose.model('User', userSchema);
module.exports = User;
```

## Work Flow

1. **Define Schema:** Create a schema that defines the structure of your data.
2. **Create Model:** Use the schema to create a model. The model is a JavaScript class that represents a collection in the database.
3. 1. **Use Model in Routes/Controllers:** In your Express routes or controllers, use the model to perform CRUD (Create, Read, Update, Delete) operations on your data.

```js
const express = require('express'); 
const router = express.Router();
const User = require('../models/User'); 
router.post('/users', async (req, res) => { 
try {     
const newUser = new User(req.body);
const savedUser = await newUser.save();
res.status(201).json(savedUser); 
} catch (error) { 
res.status(500).json({ error: error.message });  
} });  module.exports = router;`
```

This route uses the `User` model to create a new user based on the data in the request body and then saves it to the database.
In summary, schemas define the structure of your data, and models provide a programmatic interface for interacting with the data in the database. This separation helps organize and modularize your code when working with databases in Express.js applications.

