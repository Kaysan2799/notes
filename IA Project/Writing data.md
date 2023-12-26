- First we take things simple write create user code as:
```js
const express = require('express');
const router = express.Router();
const User = require('../models/User');

router.post('/createUser', async (req, res) => {
    try {
        await User.create({
            firstName: req.body.firstName,
            lastName: req.body.lastName,
            email: req.body.email,
            Gender: req.body.Gender,
            password: req.body.password
        })
        res.json({ success: true });
    }
    catch (error) {
        console.log(error)
        res.json({ success: false });
    }
});
module.exports = router;
```

- In above code we create a post method for posting signup data in database so we use the schema we have created and [[Data Handling#Work Flow| Router ]] user data as a model or schema.  
- Now in thunder client click on new request.
- Add third header as **`content type - application/json`** and   then in body write json data to post.
- Now it's time to create a request so go to URL bar and add URL ***`localhost:portno/api/createUser`*** as in above code.
- By click on send you get a response  and here you can find that your API is working or not.