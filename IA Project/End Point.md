C### API End Point
>```js
router.post('/login',
body('email', "Your email is incorrect").isEmail(),
body('password', "your Password is Incorrect").isLength({ min: 5 }),
async (req, res) => {
const result = validationResult(req);
>      if (!result.isEmpty()) {
        return res.status(400).json({ errors: result.array() });
        }
>     let email = req.body.email
        try {
          let UserData = await User.findOne({ email }){
        if (!UserData) {
          return res.status(400).json({ errors: "Incorrect Email Id" })
        }
        if (req.body.password === UserData.password) {
          res.json({ success: "true" })
        }
        else {
          return res.status(400).json({ errors: "Incorrect Password" })
          }
         }
        } 
        catch (error){
            console.error(error);
            res.status(500).json({ success: false, error: 'Internal Server Error' });
            }
    });
>```

>[!FAQ] Explanation
>- Above code is for API endpoint ***`/api/login`*** 
>- [[Express Validator#Sanitizing inputs[​](https //express-validator.github.io/docs/guides/getting-started sanitizing-inputs "Direct link to Sanitizing inputs")|Validation]] done by code first then search Data in data base and save in a ***`UserData`*** variable after that if data exist in database then perform check on password  and generating response according to situation.   

