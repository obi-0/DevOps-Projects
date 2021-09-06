### MERN PROJECT

Implemented a web solution based using MERN Stack.
MERN stack consists of the following applications and program:
•	MongoDB: A document-oriented database. It is classified as a NoSQL database program.
•	Express JS: A web application framework for node.js. It is designed for building web applications and APIs.
•	ReactJS: An open source JavaScript library used for building user interface (UI).
•	Node JS: A back-end JavaScript runtime environment  executes JavaScript code outside a web browser. 
 
The objective of this project is to deploy a To-Do application that creates To-Do lists.
Screenshot

#### STEP 1 – BACKEND CONFIGURATION

Updated Ubuntu server - `sudo apt update`
Upgraded Ubuntu – `sudo apt upgrade`

Node.js Installation
To obtain Node.js software location from Ubuntu repositories, I ran this command
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

Screenshot

![nodejs](images/nodejs.JPG)


Ran the command below to install npm. NPM is package manager used to install Node modules & packages and to manage dependency conflicts.

sudo apt-get install -y nodejs

Verified that node.js and npm successfully installed by running command
node -v 
npm -v 

Application Code Seup
Created a directory for my To-Do project
mkdir Todo

Entered command `npm init` to initialise my project. A new file package.json was created


![npm](images/npm.JPG)


#### Install ExpressJS

Ran the command below to install ExpressJS
npm install express

Created a file index.js with the command
touch index.js

Installed dotenv module by running 
npm install dotenv

Next, I opened the index.js file with command vim.index.js

Entered the code below into the file and saved using vim editor.

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

To verify that the server is running, I ran node index.js command. 

Screenshot below:



![indexjs](images/indexjs.JPG)


I created an inbound rule and opened TCP port 80 and a custom TCP port 5000 as shown below:

![port](images/port.JPG)



I opened my virtual server IP Address on my computer web browser. The web page message is shown below


![express](images/express.JPG)



My To-do list application was setup to do the following:
1.	Create a new task
2.	Display all tasks
3.	Delete completed task

Created routes which my ‘To-do’ application will depend on.

I also created a folder called routes.
Inside the routes folder, i created a file api.js, by entering command
touch api.js

Opened the file with the vim editor and entered the code below:

const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;

Saved the file `:w` and quit `:q` the vim editor

MODELS

Created a model to make the application interactive. The model will be used to define the database schema.
The Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. These are known as virtual properties.

I created a Schema and a model and installed mongoose (Node.js package).

$ npm install mongoose

Created a folder called models and created a file and named it todo,js
Opened the file and entered the code below using the vim command

`const mongoose = require('mongoose');
const Schema = mongoose.Schema;`

`//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})`

`//create model for todo
const Todo = mongoose.model('todo', TodoSchema);`

`module.exports = Todo;`

`Updated my routes from the file api.js in the ‘routes’ directory to make use of the new model.
Entered the routes directory `cd routes` 
Opened api.js with vim api.js, deleted the code `:%d` , then entered the code below. Saved and closed the file.`

`const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');`

`router.get('/todos', (req, res, next) => {`

`//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});`

`router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});`

`router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})`

`module.exports = router;`


MONGO DATABASE
To implement MERN STACK, a database is required to store my data. I utilized mLab a cloud database service to host my MongoDB Database.
I created an account on myLab (www.mongodb.com), signed in and created a database in Mongo DB
Created .env file in my Todo directory
Inside the file entered the connection string required to connect to the database
`mongodb+srv://obi:Mypassword@cluster0.2xe85.mongodb.net/myFirstDatabase?retryWrites=true&w=majority`

Updated index.js file to make use of the .env file, This will allow Node.js to connect to my database.

Updated the index,js file with the code below


`const express = require('express');`

`const bodyParser = require('body-parser');`
`const mongoose = require('mongoose');`

`const routes = require('./routes/api');`

`const path = require('path');`
`require('dotenv').config();`

`const app = express();`

`const port = process.env.PORT || 5000;`

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

`app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});`

`app.use(bodyParser.json());`

`app.use('/api', routes);`

`app.use((err, req, res, next) => {
console.log(err);
next();
});`

`app.listen(port, () => {

`console.log(`Server running on port ${port}`)`

});



Ran command node index.js to start my database server. My Database connected successfully, as shown in the image below.


![dbase](images/dbase.JPG)


I used Postman to test and verify that the API endpoint is working.
 The objective is to send a new task to our To-Do list so the application could store it in the database.

![POST](images/POST.JPG)




Created a POST request to the API, by opening my server URL followed by :5000/api/todos
The request sent a new task to my To-Do list, the application was stored in the database.




![postman](images/postman.JPG)



Created a GET request to my API on http://<PublicIP-or-PublicDNS>:5000/api/todos. 
This request retrieves all existing records from my To-do application. 



![GET](images/GET.JPG)



With Postman I was able to perform the following tasks
1.	Display a list of task - HTTP GET request
2.	Add a new task to the list – HTTP POST request




STEP 2 – CREATING THE FRONTEND

The objective of this task is to create a user interface for a web browser to interact with the application through an API. 
The ‘create-react-app’ command was used to define how the app will work.
This command created a folder called client, where react code will be added.
Prior to testing the React app, I installed some dependencies - concurrently and nodemon by running the command below.

To start the process, i ran comman 
`npx create-react-app client` to create a folder in my Todo directory called client. The client folder is where i will add my react code.

I also installed concurrently by running the command below. 

`npm install nodemon --save-dev`

Concurrently is used to run more than one command simultaneously from the same terminal window

Installed nodemon,which is used to run and monitor the server.

`npm install nodemon --save-dev`

In Todo folder, i opened the package.json file. Change the highlighted part of the below screenshot and replace with the code below.
Inside the client folder amended the` packet.json` file by entering 

"proxy": "http://localhost:5000".

This is to access the application directly from the browser by simply calling the server url like http://localhost:5000

Ran command `npm run dev` in the Todo directory, to verify that my app will open and start running on localhost:3000
Finally opened TCP port 3000 on my EC2 instance.

Screenshot showing the database connected successfuly.


![lh3000](images/lh3000.JPG)


Screenshot showing access from my web browser.


![React](images/React.JPG)







#### Creating my React Component

Two stateful components and one stateless component were setup.
While in the Todo/src directory, i setup a folder called components

`mkdir components`


In the components directory created three files by running the command below

`touch Input.js ListTodo.js Todo.js`

Opened Input.js file and entered the code below:

`import React, { Component } from 'react';
import axios from 'axios';`

`class Input extends Component {`

`state = {
action: ""
}`

`addTodo = () => {
const task = {action: this.state.action}`

    `if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
})
}

render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>

)

}

}

`export default Input`

Inside the src/clients folder, i installed Axios.
Axios is a Promise based HTTP client for the browser and node.js

`npm install axios`

Opened the Axiom folder and installed axios.

In the src/components folder, i opened ListTodo.js file and typed in the code below.


import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {

return (

<ul>

{

todos &&

todos.length > 0 ?

(

todos.map(todo => {

return (

<li key={todo._id} onClick={() => deleteTodo(todo._id)

}>{todo.action}</li>

)

})

)

:
(

<li>No todo(s) left</li>

)

}

</ul>

)

}

export default ListTodo


Opened Todo.js file and entered the code below:

import React, {Component} from 'react';

import axios from 'axios';

import Input from './Input';

import ListTodo from './ListTodo';

class Todo extends Component {

state = {

todos: []

}

componentDidMount(){

this.getTodos();
}

getTodos = () => {

axios.get('/api/todos')

.then(res => {

if(res.data){

this.setState({

todos: res.data

})

}

})

.catch(err => console.log(err))

}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {

let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;

In the src folder, opened the App.js file and deleted the code and then entered the code below.

import React from 'react';

import Todo from './components/Todo';

import './App.css';

const App = () => {

return (

<div className="App">

<Todo />

</div>

);

}

export default App;


Opened the App.css file

`vi App.css`

Entered the code below

.App {

text-align: center;

font-size: calc(10px + 2vmin);

width: 60%;

margin-left: auto;

margin-right: auto;

}

input {

height: 40px;

border: none;

border-bottom: 2px #101113 solid;

background: none;

color: #787a80;

}

input:focus {

outline: none;

}

button {

width: 25%;

height: 45px;

border: none;

margin-left: 10px;

font-size: 25px;

background: #101113;

border-radius: 5px;

color: #787a80;

cursor: pointer;

}

button:focus {

outline: none;

}

ul {

list-style: none;

text-align: left;

padding: 15px;

background: #171a1f;

border-radius: 5px;

}

li {

padding: 15px;

font-size: 1.5rem;

margin-bottom: 15px;

background: #282c34;

border-radius: 5px;

overflow-wrap: break-word;

cursor: pointer;

}

@media only screen and (min-width: 300px) {

.App {

width: 80%;

}

input {

width: 100%

}

button {

width: 100%;

margin-top: 15px;

margin-left: 0;

}

}

@media only screen and (min-width: 640px) {

.App {

width: 60%;

}

input {

width: 50%;

}

button {

width: 30%;

margin-left: 10px;

margin-top: 0;

}

}

Still in the src folder, opened the index.css file and entered this code.

body {

margin: 0;

padding: 0;

font-family: -apple-system, BlinkMacSystemFont, 
"Segoe UI", "Roboto", "Oxygen",

"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", 

"Helvetica Neue",

sans-serif;

-webkit-font-smoothing: antialiased;

-moz-osx-font-smoothing: grayscale;

box-sizing: border-box;

background-color: #282c34;

color: #787a80;

}

code {

font-family: source-code-pro, Menlo, Monaco, 

Consolas, "Courier New",

monospace;

}



Finally, in the $Todo directory, ran the script below to start Express.js server. 

`npm run dev` 

I can now open my Todo app on my web browser - 

URL: virtual server IP Address followed by :3000

On my web browser, i added a new task 'Completed project 3', as shown in the image below.



![Todo](images/Todo.JPG)

 







