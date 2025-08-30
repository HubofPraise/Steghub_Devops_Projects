## MERN WEB STACK IMPLEMENTATION ##
### OVERVIEW ###

The MERN stack is a collection of four technologies that work together to build modern web applications, from the user interface down to the database. Each component plays a key role:
- MongoDB – A NoSQL database that stores data in flexible, document-like structures (similar to JSON). Think of it as the storage box for user details, posts, products, and more.
- Express.js – A server-side framework for Node.js that handles requests and responses. It acts like the messenger, connecting the user’s actions to the database.
- React.js – A frontend library developed by Facebook. It builds dynamic and interactive user interfaces—the buttons, forms, and pages users see. In simple terms, the designer of the app.
- Node.js – A runtime environment that allows JavaScript to run on the server (not just the browser). It’s the engine powering the backend.

### HOW THEY WORK TOGETHER ####
Here’s the flow of a MERN application:
- User Interaction – The user clicks a button or submits a form on the React.js frontend.
- Request Handling – That action is sent to the server, where Express.js running on Node.js decides what to do.
- Data Processing – If the action needs data, the server talks to MongoDB to fetch or save information.
- Response Delivery – The server sends the results back, and React.js updates the interface instantly so the user sees the changes.


*In short: React shows it, Express handles it, Node powers it, MongoDB stores it.*


### STEP 1: LAUNCH AN EC2 INSTANCE ###
Before starting this project, you will need access to:

- An AWS account (Free Tier recommended).
- A virtual server running Ubuntu Server OS.

If you don’t already have an AWS account, refer back to 'LAMP STACK IMPLEMENTATION' for instructions on signing up. Once signed in, create a new EC2 Instance using the following:

Instance type: t2.micro (Free Tier eligible).

Image (AMI): Ubuntu Server 24.04 LTS (HVM).

⚠️ Tip: You can create multiple EC2 instances for different projects, but always STOP the ones you are not actively using to stay within the Free Tier limit.

💡 Hint: When launching your EC2 instance, add a Tag with:

Key: Name
Value: (e.g., MERN-Project-Server)

### STEP 2: BACKEND CONFIGURATION ###

Update Ubuntu:

```bash
sudo apt update
```
Upgrade Ubuntu:

```bash
sudo apt upgrade
```

Get the location of Node.js software from Ubuntu repositories:

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```


Install Node.js on the Server:

```bash
sudo apt-get install -y nodejs
```

Verify the node installation:

```bash
node -v
npm -v
```

Application Code Setup:
Create a new directory for your To-Do project.

```bash
$ mkdir Todo
```
Verify it was created:

```bash
$ ls
```
Change the current directory to the newly created one:

```bash
$ cd Todo
```
Run the command to initialize the project and verify that it was created:

```bash
$ npm init
$ ls
```
A package.json has been created.

### STEP 3: INSTALLING EXPRESSJS ###

```bash
$ npm install express
```
Express helps to define routes of your application based on HTTP methods and URLs.

Create a file index.js with the command below:

```bash
$ touch index.js
```
Recall to verify the file has been created.

Install the dotenv module:

```bash
$ npm install dotenv
```

Open the index.js file:

```bash
$ vim index.js
```

Input the code into it:

```bash
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});

```
Notice that we have specified to use port 5000 in the code. Just so you know, this will be required later when we go to the browser.

- Use :w to save in vim 
- Use :qa to exit vim

Open the terminal in the same directory as the index.js file and type:

```bash
$ node index.js
```
*Now open this port in EC2 security Groups. Create an inbound rule to open TCP port 80, same as port 5000*

On browser:

```web
http://<publicip>:5000
```
### DEFINE APPLICATION ROUTES ###

Our To-Do application needs to perform three key actions:

- Create a new task – handled by the POST method.
- Display all tasks – handled by the GET method.
- Delete a completed task – handled by the DELETE method.

Each action will correspond to a specific endpoint in the backend. To organize these endpoints properly, we’ll create a dedicated folder for routes.

Create Folder 'routes':

```bash
$ mkdir routes
```
Change directory routes folder:

```bash
$ cd routes
```

Create a file named api.js:

```bash
$ touch api.js
```

Open the file:

```bash
$ vim api.js
```

Copy the code below into the file:

```bash
const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;
```

### STEP 4: MODEL ###
Connecting our app to MongoDB. Since MongoDB is a NoSQL database, we need a way to define how our data (tasks) should look and behave. This is where models come in.

What is a Model?
- A model represents the structure of your data inside the database.
- It uses a Schema, which is like a blueprint describing what fields each document (record) will have.
- For example, a “task” might have fields like title, description, or status.


Change directory back to the Todo Folder:-

Installing mongoose:
```bash
cd Todo
$ npm install mongoose
```

Create a new folder 'models', change its directory, and create a file  inside the Todo Folder:
```bash
mkdir models && cd models && touch todo.js
```
*All three commands above can be defined in one line to be executed consecutively with the help of the && operator*

Open the file created with vim:
```bash
vim todo.js
```
*copy and paste the code inside*
```bash
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```
*Now we need to update our routes from the file api.js in 'routes' directory to make use of the new model.*

In the Routes directory, open api.js with vim api.js, delete the code inside with :%d command, and paste the code below into it, then save and exit

```bash
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})

module.exports = router;

```

### STEP5: MONOGODB DATABASE ###

In order for our To-Do application to store data, we need a database. For this, we’ll use mLab (now part of MongoDB Atlas), which provides MongoDB as a Database-as-a-Service (DBaaS) solution. This saves us the stress of installing and managing MongoDB locally.

Steps to Create a Free Database
Go to mLab (MongoDB Atlas) and sign up for a free account. Sign Up Here

During setup:
- Choose Cloud Provider: Select AWS.
- Select Region: Pick a region closest to you for better performance.
- Cluster Tier: Choose the Shared Cluster (Free Tier) option—it’s perfect for our use case.

Once your cluster is created, you’ll be able to:
- Create a Database User (with a username and password).
- Get a Connection String (a special URL used by Mongoose to connect your app to the database).
- Copy this connection string and keep it safe—you’ll use it soon to link your Node.js app with MongoDB.














