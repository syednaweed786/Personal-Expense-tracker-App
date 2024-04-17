To create a database using MongoDB and Mongosh, you'll follow these steps:

1. *Install MongoDB:*
   - Download and install MongoDB from the official MongoDB website (https://www.mongodb.com/try/download/community).
   - Follow the installation instructions for your operating system.

2. *Start MongoDB:*
   - After installing MongoDB, start the MongoDB server. The default port for MongoDB is 27017.
   - You can start MongoDB from the command line by running mongod.
   - If MongoDB is installed as a service, it may start automatically.

3. *Connect to MongoDB:*
   - Open a new terminal window and start the Mongosh shell by running mongosh.
   - By default, Mongosh will attempt to connect to the MongoDB server running on localhost at port 27017.
   - If MongoDB is running on a different host or port, you can specify the connection URI as an argument to mongosh. For example:
     
     mongosh mongodb://localhost:27017/mydatabase
     

4. *Create a Database:*
   - To create a new database, you can use the use command followed by the name of the database you want to create. For example:
     
     use mydatabase
     
   - This command will switch to the mydatabase database if it exists. If the database does not exist, MongoDB will create it for you when you insert data into it.

5. *Create Collections:*
   - MongoDB stores data in collections, which are analogous to tables in relational databases.
   - You can create a new collection by inserting data into it using the insertOne or insertMany commands. For example:
     
     db.books.insertOne({ title: "Book 1", author: "Author 1" })
     
   - This command will create a new collection named books and insert a document into it with the specified title and author fields.

6. *View Databases and Collections:*
   - You can view a list of all databases using the show dbs command.
   - You can switch between databases using the use command.
   - You can view a list of collections in the current database using the show collections command.

7. *Query Data:*
   - You can query data from a collection using the find command. For example:
     
     db.books.find()
     
   - This command will retrieve all documents from the books collection.

8. *Exit Mongosh:*
   - To exit the Mongosh shell, you can type exit or press Ctrl + D.

That's it! You've now created a database using MongoDB and interacted with it using the Mongosh shell. You can continue to insert, update, delete, and query data as needed to build your application.
