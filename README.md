# W2D5 - Lecture

## MongoDB

* Intro to databases (Why)
* Intro to MongoDB
  * Server -> Databases -> Collections -> Docs
  * 
  * MongoDB from Node (demo)

### Without actual databases

Currently in our project we have a "global" object in memory to track our shortened URL records: 

```javascript
var urlDatabase = { 
  "abc": "http://",
  "def": "http://"
} 
```

The biggest problem is that this data goes away when we restart our node process (something that we must assume can happen any time for many reasons).

This happens because the data is being stored IN MEMORY and not ON DISK. 

Memory is (meant to be) volatile storage whereas disk (file system) is long term storage.

We need to serialize and persist this object to  disk ("file system").

Either binary are text files are often used to store long term info.

Ideally our app will persist this object on disk and then read/write from that file. 

### Flat files

Flat text files (like CSV files) are the simplest but least powerful approach.

Comma separated values and it's just a text file that has each record on one line separated by commas. 

```
"abc", "http://"
"def", "http://"
```

### JSON (or similar) format files

Another example of a textual file that can store our data is a `json` file. JSON is another approach to serializing our data (for purposes of storage or transmission), and isn't totally flat (has nested objects, etc) which is great... but doesn't solve other more critical challenges.

Example of a JSON file:

```javascript
{ 
  "abc": "http://",
  "def": "http://"
} 
```

#### Challenges 

Challenges with storing and managing our own text (CSV, JSON, etc) files for user data: 

- Too much work on the app developer to work with this file (Open, Read, Write, etc)
- No way to easily search data by criteria
- Concurrency: multiple apps/processes can't read/write the file without causing inconsistency. Web apps can have many users CRUDing data and so we need to be able to read/write concurrently without problems. 

For reasons above, text files are not used for application data BUT are used for static / non-user data like configuration settings:

Examples:

- NPM has `package.json`
- Git has `.gitignore` and others 
- For app settings we used a simple `.env` key-value file


### Enter MongoDB (or other alternatives)

MongoDB is a document storage database. It has blown up big time in the last in the Node community, becoming the most popular option for many projects.

Disclaimer: I can't speak to the accuracy of this list but it does give us _some_ idea: 

<http://db-engines.com/en/ranking>

MongoDB is open source (like everything we use/teach here). It is a non-relational db and is more of an object store (objects are referred to as "documents").

The major components of Mongo are:

```
- Databases (Most apps usually have one)
  - Collections (think of it like an Array)
    - Documents (JS Objects with _id's)
```

A Mongo server has many Databases which have many collections within and these collections within have many documents.

We can store whatever we want in the documents. Here is an visual example of a todo app's mongo database, so we can see how we would use these different entities / layers to structure the data for an app like the one we are building this week:

```
- Database: todo_app_db
  - Collection: todos
    - Doc: {desc: '...', completed: true}
    - Doc: {desc: '...', completed: false}
    - Doc: {desc: '...', completed: true}
  - Collection: users
    - Doc: {username: 'kvirani', ...}
    - Doc: {username: 'rafd', ...}
    - Doc: {username: 'pjama', ...}
```

## Transactional functions/operations

Mongo has functions on collections to allow us to CRUD (Create Read Update Delete) documents.

## Demo 1: Mongo shell / REPL

We went over the mongo shell. Here are the commands we ran:

```javascript
show dbs
use todos_app
db
db.todos
db.todos.insert({ desc: 'Example TODO', completed: false })
db.todos.insert({ desc: 'Example Completed TODO', completed: true })
db.todos.find()
db.todos.find({completed: true})
db.todos.find({id: ObjectId("unique-mongo-id-here")})
```

We talked about how:

- Mongo is very JS-centric. Most of the code we see in the shell above is JS code!
- There are many other functions that you can look up in the documentation (how to search, insert multiple, delete, update, etc)
- Mongo assigns an "_id" key/property to any document so we can uniquely identify it
- Collections can just be viewed as properties of the `db` object (`db.todos` can also be written as `db.collection('todos')`) therefore their name is what uniquely identify them.

## Primary Key

The `"_id"` is a "Primary Key" that uniquely identifies each object (document) within our collections. 

It is automatically added to any object we insert and we can use it to find single object from a collection (see example above).

## Demo 2: Mongo shell / REPL

We walked through the Todo Node/Express app which only has index, new/create and delete functionality. It uses Mongo as its data store and we went through the `app.js` file in detail to see how it does it.


## Client vs Server

So far, we're used to seeing our node / express app as a Server when it comes to HTTP requests from the browser (Client). 

In terms of data access and Mongo, the node/express app is actually the Client and Mongo is the Server.

```
Browser      (Client) <> Node/Express (Server)
Node/Express (Client) <> MongoDB (Server)
```

So together we have:

```
Browser      (Client) <> Node/Express (< Server / Client >) <> MongoDB (Server)
```





