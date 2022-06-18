### MERN STACK IMPLEMENTATION
# SIMPLE TO-DO APPLICATION ON MERN WEB STACK
## MERN Stack is a collection of powerful technologies and robust example of Web Stack Technology, which is used to develop a scalable and well-functioning software product comprising backend, front-end, and database.
### •	MERN (MongoDB, ExpressJS, ReactJS, Node.js)
#### STEP 1
   I started by launching a new instance on my AWS Console and choose Ubuntu version 22.04.LTS.

   I created a new key pair for my instance and click on connect to copy my generated SSH client

Platform :
    Ubuntu 22.04.LTS

Platform details:
    Linux/UNIX

#### STEP 2

   I connected to my EC2 instance by running

    “ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>”

from my terminal to create my Linux server in the cloud.
 
 #### STEP 3 

   I opened my Ubuntu terminal and typed “sudo apt update” and “sudo apt upgrade” to update and upgrade each out-dated dependencies and packages on my system.

![sudo apt update](https://user-images.githubusercontent.com/79808404/174454632-7410ec17-efa1-4ff1-ad1e-2a8d4de8a726.PNG)

## BACKEND CONFIGURATION
#### STEP 1  
    I ran
         “curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -" 
To get location of Node.js from my Ubuntu repo
![getting location for my nodejs from ubuntu repo](https://user-images.githubusercontent.com/79808404/174454871-239cb79c-1d09-48cd-962b-3df1e565fe19.PNG)

#### STEP 2
  I installed Node.js and npm on my server with the command below
          "sudo apt-get install -y nodejs"
	  
   ![nodejs install](https://user-images.githubusercontent.com/79808404/174454881-5e7d39ee-7f3c-4791-bf75-8a30c7e3f91a.PNG)


#### STEP 3
  I checked the version of the node and npm installed to be sure they are installed as expected by running 
    node -v 
    npm -v 
    
![node version](https://user-images.githubusercontent.com/79808404/174454928-1600c68f-155b-4ad0-9d7e-a84577169566.PNG)

  #### STEP 4
     I created a Todo directory and cd into it with the command
   " mkdir Todo" and   "cd Todo"
   
![make todo dir and cd](https://user-images.githubusercontent.com/79808404/174454957-246f5b97-a528-436b-a887-0134cc78dc2b.PNG)

#### STEP 5
  I created a package.json file to secure my file by running 
     "npm init"
  and ran the Ls command to confirm that the package.json file is created successfully.
  

![npm init2](https://user-images.githubusercontent.com/79808404/174454998-5ced635f-862d-44d5-9c29-08955070d247.PNG)


## INSTALL EXPRESSJS which is a framework for Node.js

 #### STEP 1 
  I installed express in my server by running 
        "npm install express"
	
  ![npm install express](https://user-images.githubusercontent.com/79808404/174455177-444d23ce-dd20-4239-8569-a3fb31e842b4.PNG)


#### STEP 2
  I created an index.js file by running 
  " touch index.js"
 and ran Ls command to confirm that the file is created successfully
 
#### STEP 3
  I installed dotenv module by running
    "npm install dotenv"
    
  ![install dotenv](https://user-images.githubusercontent.com/79808404/174455207-4b4cbe3d-6dcd-4f08-bd87-d81bdb57de22.PNG)


  #### STEP 4
   I opened my index.js file with vim command
       "vim index.js"
   and I inserted the code below and specified to use port 5000 in the index.js file
   
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
																																									
																															

Then I click the “esc” button and use “:w” to save the file and “:qa” to exit . 

#### STEP 5
   I checked if my server is running on port 5000 as expected by running
        " node index.js"
	
  ![indexjs on port 5000](https://user-images.githubusercontent.com/79808404/174455336-9ab1c1a0-a32f-4039-871c-3e4433a3c412.PNG)


#### STEP 6  
   I opened my AWS EC2 security group to edit the inbound rule and include port 5000
Then I typed http://<my_PublicIP>:5000  into my web browser to confirm if my server is running as expected.

![welcome to express](https://user-images.githubusercontent.com/79808404/174455396-67827404-7f2a-4914-924f-9f94cc7ed082.PNG)


## MODEL

#### Step 1
   I changed my directory back to my Todo directory with cd.. and I installed MongoDB as my database by running
        "sudo npm install mongoose"
	
 ![npm install mongoose](https://user-images.githubusercontent.com/79808404/174455623-08b651a0-39b5-45a8-b53a-44bb30dda3f1.PNG)

#### STEP 2
   I created a new directory Models and cd into the directory
       " mkdir models" " cd models"
       
#### STEP 3
   I created a todo.js file inside the models folder
          "touch todo.js"
 #### STEP 4
   I opened the todo file I created by running
          “Sudo vim todo.js”	and I input the below script in the file and saved  and exit the file by typing “esc key” and “:wq”

       "const mongoose = require('mongoose');
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
 
#### STEP 5 
   I updated my “api.js” file in routes directory to make use of the new model by deleting the code inside with “ :%d” and installing the new command below.
       
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

    module.exports = router; "

Then I  saved  and exit the file by typing “esc key” and “:wq”



## MONGODB DATABASE
#### STEP 1
  I Created an account on mLab which provides MongoDB database as a service solution.
I selected AWS as a cloud provider for my database and I clicked on get started which gave me some checklist to complete before completing  my  mLab account
I changed the deleting time of entry to 1 week.

I clicked on cluster then navigate to collections where I clicked on “Add my Own File”

#### STEP 2
I cd into my Todo directory and I created a .env file in my Todo directory by running
                         "touch .env"
			 
 ![create a env file](https://user-images.githubusercontent.com/79808404/174456240-804951d7-0f12-4582-aa8c-5488b1537481.PNG)

                          
 I ran "vi.env" to add a connection string to access my database by typing the below in my command
   
    DB = 'mongodb+srv://myDatabaseUsername:myDatabasePassword@my-Mongodb-network-address/myDatabaseName?retryWrites=true&w=majority'

#### STEP 3
  I opened  my MongoDB database and navigate to Database deployments and I clicked on connect to connect my application to my database by copying the generated link
 
 ![database cont to cluster](https://user-images.githubusercontent.com/79808404/174456272-339b5054-5184-4943-a2b5-98a031018618.PNG)

#### STEP 4
 I opened my index.js file and by typing "Vim index.js" and input the code below and save and exit the file.
     
     const express = require('express');
    const bodyParser = require('body-parser');
    const mongoose = require('mongoose');
    const routes = require('./routes/api');
    const path = require('path');
    require('dotenv').config();
  
    const app = express();

    const port = process.env.PORT || 5000;

    //connect to the database
    mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
    .then(() => console.log(`Database connected successfully`))
    .catch(err => console.log(err));

    //since mongoose promise is depreciated, we overide it with node's promise
     mongoose.Promise = global.Promise;

    app.use((req, res, next) => {
    res.header("Access-Control-Allow-Origin", "\*");
    res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
    next();
    });

    app.use(bodyParser.json());

    app.use('/api', routes);

    app.use((err, req, res, next) => {
    console.log(err);
    next();
    });

    app.listen(port, () => {
    console.log(`Server running on port ${port}`)
    });
   
#### STEP 5
   I started my server by running the command below to test if my configuration is working as expected
       "node index.js"
       
![database connectd](https://user-images.githubusercontent.com/79808404/174456473-35958e61-5905-4fa3-99d0-120b69c0c48c.PNG)

## Testing Backend Code using REST API(POSTMAN)

  #### STEP 1
 In my API workspace, I created a Post and Get request with my Public IP,
 running on port 5000. These requests will send a new task to my To-Do list so my application could store it in the database  

![apiPost](https://user-images.githubusercontent.com/79808404/174456504-770811be-6324-4b6b-8ba4-afe18653140b.PNG)
![apiGet](https://user-images.githubusercontent.com/79808404/174456526-83f9ce6f-92fc-443f-9fc0-e8267abff1f2.PNG)

#### STEP 2
   I set my header key content application-type as Json , I navigated to body section and input a message in the body to test my application.
---------------------------------------------------------------------------------------------------------

  ## FRONTEND CREATION
    
 #### STEP 1
   In my Todo directory, I ran npx create-react-app client to create  a new folder in my Todo directory where all my react code will be.
 
   ![npx create react](https://user-images.githubusercontent.com/79808404/174456602-54ab7f78-f5ae-40ce-bcc7-3a0d62a8c2fe.PNG)

#### STEP 2
I installed the below dependencies _concurrently_(to run more than one command simultaneously from the same terminal ) and _nodemon_(to run and monitor my server for any changes in my server code) by running
       "npm install concurrently --save-dev"
      " npm install nodemon --save-dev"



#### STEP 3
  In my Todo folder, I opened the package.json file and input the script below
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
![packageJson edit](https://user-images.githubusercontent.com/79808404/174456741-3adae4d0-4864-4e9d-88d8-e307a8eb3c78.PNG)


#### STEP 4
   I cd into Client directory and opened the package.json file by running vi package.json and I included the below config
             "proxy": http://localhost:5000

#### STEP 5
 In my AWS Console, I added a new security group rule to open TCP port 3000
  in my Todo directory, I ran npm run dev command which confirmed that my app is opened and running on port: 3000.
![React3000](https://user-images.githubusercontent.com/79808404/174457583-826d97ce-a39f-48b0-8c92-3351f8e932dd.PNG)



## Creating React Components

 #### STEP 1 
From my Todo directory, I cd into client/src and created a new directory in the src folder 
          mkdir components
 I cd into components and created the files below in the components folder
     touch Input.js ListTodo.js Todo.js
 
#### STEP 2
 
   I opened the input.js file  by "vi input.js" and input the below script
      
      import React, { Component } from 'react';
    import axios from 'axios';

    class Input extends Component {

    state = {
    action: ""
    }

    addTodo = () => {
     const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
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

      export default Input
#### STEP 2
  I installed Axios in my client directory which is a promise based HTTP client for node.js and the browser.
    "npm install axios"
    
![npm install axi](https://user-images.githubusercontent.com/79808404/174457672-337b7cef-f4c9-4d1b-9129-7a7a4d001781.PNG)


#### STEP 3
   After the installation of Axios on my server, I cd back into the components directory and I opened ListTodo.js file by running vi ListTodo.js and input the below code
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
    <li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
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
    
#### STEP 4
Then I opened my Todo.js file in the same components directory and input the below code
   
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


#### STEP 5
  Making an adjustment to my react app by removing the default react logo and inputting a new code
   I cd back into the src folder and opened the App.js by running vi App.js and input the code below, then saved the file and exit.
         
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

STEP 6
  In the same src directory, I opened the App.css file and input the following code
      
      .App {
      text-align: center;
      font-size: calc(10px + 2vmin);
      width: 60%;
      margin-left: auto;
      margin-right: auto;
      }

      input {
      height: 40px;
      width: 50%;
      border: none;
      border-bottom: 2px #101113 solid;
      background: none;
      font-size: 1.5rem;
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
![appCss](https://user-images.githubusercontent.com/79808404/174457716-95860a56-6a0c-4d00-9552-5b5cb0776ddb.PNG)

#### STEP 7
  Still in the src directory, I opened the index.css file by running vim index.css
and entered the code below
   
    body {
    margin: 0;
    padding: 0;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
    "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
    sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    box-sizing: border-box;
    background-color: #282c34;
    color: #787a80;
    }

    code {
    font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
    monospace;
    }
  ![indexCss](https://user-images.githubusercontent.com/79808404/174457702-07f97ece-79e9-47be-a8ed-b7a0cb534da8.PNG)


#### STEP 8
   Then I cd back to Todo directory “cd ../..” and ran npm run dev
   To view my App is running as expected on the localhost port:3000

 
![npmRunDev3000](https://user-images.githubusercontent.com/79808404/174457383-dd1066bd-a844-4c11-885b-db19427c7822.PNG)



## CHALLENGES ENCOUNTERED
 When i ran node index.js to see if database is connected and running on port 5000 as configured but i got the below error instead
 ![error 1](https://user-images.githubusercontent.com/79808404/174457830-a0de07ac-908f-4026-80f3-77b8571a8ed0.PNG)

## SOLUTION
 After spending couple of hours trying to debug what might have caused the error, i checked on slack overflow and got a hint that the error was becaused i've included a special character in database password.
![database connectd](https://user-images.githubusercontent.com/79808404/174457968-f2560a2f-baad-4a9f-b0bb-8948c1c99dab.PNG)
