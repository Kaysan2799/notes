>[!Tip]- Index File (Express) 
>```js
const express = require('express')
const app = express()
const port = 5000
const mongoose = require('mongoose')
const server = require('./db')
server()
app.get('/', (req, res) => {
res.send('Hello World!')
})
app.use(express.json())
app.use('/api', require('./Routes/CreateUser'));
app.listen(port, () => {
console.log(`Example app listening on port ${port}`)
})


>[!Tip] **Express Setup:**
`const express = require('express') const app = express() const port = 5000`  
>    - Import the `express` module and create an Express application (`app`).
  >  - Define the port number (`5000`) that the server will listen on.


>[!Tip] **MongoDB Connection:**
`const mongoose = require('mongoose') const server = require('./db') server()`
>  - Import the `mongoose` module.
  >  - Import the `server` function from the local `./db` module and invoke it. This function likely establishes a connection to the MongoDB database.

>[!Tip]  **Express Route:**
`app.get('/', (req, res) => {   res.send('Hello World!') })`
>- Define a route for the root URL ("/"). When a request is made to the root URL, the server responds with the text "Hello World!"


>[!Tip]  **Server Start:**
``app.listen(port, () => {   console.log(`Example app listening on port ${port}`) })``
> - Start the Express server to listen on the specified port (`5000`).
  >  - Output a message to the console indicating that the server is listening on the specified port

>[!Tip]  **Express Json:**
`app.use(express.json())`

- This line adds middleware to your Express application to parse incoming requests with JSON payloads.
- `express.json()` is a built-in middleware in Express that parses incoming requests with JSON payloads and makes the parsed data available in `req.body`.
- When a client sends a POST or PUT request with a JSON payload, this middleware ensures that the JSON data is parsed and available for use in your route handlers.
- Output a message to the console indicating that the server is listening on the specified port.

>[!Tip]  **Express Json:**
`app.use('/api', require('./Routes/CreateUser'));`

- This line mounts the specified middleware function or router at the specified path (`'/api'` in this case).
- It includes a router located in the file `./Routes/CreateUser`.
- The `'/api'` prefix means that this router will only handle requests that start with `/api`. For example, if you have a route defined in `CreateUser.js` as `'/users'`, it will be accessible at `/api/users`.