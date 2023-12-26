- Download mongo-db. community server and install it as a service and then it ask to install mongo-db. shell and uncheck the compass to continue to install. 
- After that all you have to do is go to the bin path of mongo-db. Copy that path and paste it into environment Variable and restart your PC and that's it.
- Now after restart you can verify this using command prompt.
- After that you have to go to mongo-db. web page and create account.
- Then Create a project.
- Then Create a Cluster.
- Then Create a Collection.
- And that's it you have successfully create a data-base.
- Now you have to set-up few more thing go to Network access and edit it to setup.
- Now you can go to Database Access and assign user setup verification method, here you can set permissions for the users so, let's do it.
- After that you can go to Database and navigate to cluster you have created and here is a option to connect to that cluster and by this you can get your connection string.

## vs-code 
you have to create a folder out side of src directory and you have to run few commands in this directory 

>[!Command]
>``` js
> npm  init 
> and after it few files are created and then you have just here to code


[[Express]]
- To install  express package you have to run this command  
>[!Express]
>```nmp
>npm install  express 


[[Mongoose]]
- After that you have to install  mongoose which is also a package 
>[!Mongoose]
>```npm
>npm install mongoose

>[!Note ] nodemon
> ```js
> npm install nodemon
> for refresh server in the backend express 

>[!Note] Thunder Client 
>Install this extension in VS code it Will help you in testing API i.e. testing by send and receive data  request and here you can also see the responce.

- After this setup all you have to done is create two folders in backed directory.
	- Models 
	- Routes
- In model directory create a [[Data Handling#Schema & Model|Schema]] and use this schema in creating user data model so why late let's do it.
- Now create another directory  Routes and in that directory create [[Data Handling#Work Flow|User]] file and in that file you have to import schema. 
