## `Mongodb Atlas` 

>[!Tip]- Template Code 
>
>```js
const mongoose = require('mongoose');
const conn = "mongodb+srv://Atta-un-nabi:SARdar@cluster0.qia72xf.mongodb.net/Userdata?retryWrites=true&w=majority";
const connectToDatabase = async () => {
  try {
    await mongoose.connect(conn, { useNewUrlParser: true, useUnifiedTopology: true });
    console.log('Connected to MongoDB');
    const fetched_data = mongoose.connection.db.collection("logindata");
    const data = await fetched_data.find({}).toArray();
    console.log(data)
    mongoose.connection.close();
>  	} catch (error) {
   console.error('MongoDB connection error:', error);
  }
};
 module.exports = connectToDatabase;


>[!Tip] Library Import in Node js
>**`const mongoose = require('mongoose');`**
    >- Imports the Mongoose library, which is an Object Data Modeling (ODM) library forMongoDB and Node.js.

>[!Tip] Creating Connection  String 
> **`const conn = "mongodb+srv://Atta-un-nabi:SARdar@cluster0.qia72xf.mongodb.net/Userdata?retryWrites=true&w=majority";`**
    >
   > - Defines the MongoDB connection string. This string contains information about the MongoDB server, such as the username, password, cluster address, and database name.

>[!Tip] Function declaration
>**`const connectToDatabase = async () => {`**
>- Declares a function named `connectToDatabase` using the `async` keyword, indicating that the
>function will contain asynchronous operations and can use `await`.

>[!Tip] Exception Handling
> **`try { ... } catch (error) { ... }`**
>- Wraps the code in a try-catch block. The `try` block contains the code that might throw exceptions, and the `catch` block handles any exceptions that occur during the execution of the code inside the `try` block.

>[!Tip] Creating Connection
> **`await mongoose.connect(conn, { useNewUrlParser: true, useUnifiedTopology: true });`**
>- Establishes a connection to the MongoDB database using the specified connection string (`conn`). The `await` keyword is used because `mongoose.connect` returns a promise, and it waits for the promise to be resolved before moving on to the next line.

>[!FAQ] Connection Message
> **`console.log('Connected to MongoDB');`**
> - Outputs a message to the console indicating that the connection to MongoDB has been successfully established.

>[!Tip] Store response in a variable 
> **`const fetched_data = mongoose.connection.db.collection("logindata");`**
>- Retrieves a reference to the "logindata" collection within the connected MongoDB database.

>[!Tip] Retrieve data   
> **`const data = await fetched_data.find({}).toArray();`**
   > - Uses the `find` method to retrieve all documents from the "logindata" collection and convert the result to an array. The `await` keyword is used because `find` returns a cursor, and `toArray` returns a promise. Empty curly braces means all. 

>[!FAQ] Show Data on Terminal
> **`console.log(data)`**
    >- Outputs the retrieved data (an array of documents) to the console.

>[!Tip] Close Connection
> **`mongoose.connection.close();`**
    >- Closes the MongoDB connection. It's generally a good practice to close the connection when you're done working with the database.

>[!FAQ] Show Error Messege
> **` catch (error) { console.error('MongoDB connection error:', error); }`**
    >- Handles any errors that may occur during the execution of the code inside the `try` block. If an error occurs, it logs a detailed error message to the console.

>[!Tip]  Export module to use in express
> **`module.exports = connectToDatabase;`**
    > - Exports the `connectToDatabase` function, making it available for use in other modules.
