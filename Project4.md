### MEAN STACK IMPLEMENTATION
# MEAN STACK DEPLOYMENT TO UBUNTU IN AWS
### MEAN Stack is an example of WEB Stack Technology, which is a set of frameworks and tools used to develop a well-functioning software product.

  ### •	MEAN (MongoDB, ExpressJS, Angular, Node.js)

           MongoDB (Document database) – Stores and allows to retrieve data.
        •	Express (Back-end application framework) – Makes requests to Database for Reads and Writes.
        •	Angular (Front-end application framework) – Handles Client and Server Requests
        •	Node.js (JavaScript runtime environment) – Accepts requests and displays results to end user


#### STEP 1
   I started by launching a new instance on my AWS and choosing Ubuntu version 20.04.LTS

   I created a new key pair for my instance and click on connect to copy my generated SSH client

Platform :
    Ubuntu 20.04.LTS

Platform details:
    Linux/UNIX



#### STEP 2

   I connected to my EC2 instance by running my ssh key

    “ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>”

from my terminal to create my Linux server in the cloud.




#### STEP 3 

   I opened my Ubuntu terminal and typed “sudo apt update” and “sudo apt upgrade” to update and upgrade each out-dated dependencies and packages on my system.

  
#### STEP 4
   I added certificates by running the below command in my terminal

      sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates

     curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash –

![1 sudo install curl](https://user-images.githubusercontent.com/79808404/174502492-bd209d7d-da74-457d-a453-b03b1b4d9430.PNG)


#### STEP 5 
 I installed Nodejs by running
         "sudo apt install -y nodejs"
![3 sudo install nodejs](https://user-images.githubusercontent.com/79808404/174502512-18eff732-a179-4b2a-b4ec-f2970e82486c.PNG)


 ## MONGODB
 
  ### I will be installing MongoDB which stores data in a flexible, Json-like document.
 
#### STEP 1 
For a secure end to end I installed a Keyserver to for my application by running

   sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
                                                   &
   echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
![4 sudo apt key](https://user-images.githubusercontent.com/79808404/174502538-36ffe1eb-9db8-4123-897e-b886ef0309e8.PNG)

#### STEP 2

   I installed MongoDB by running 
   
             sudo apt install -y mongodb
             
![sudo install  mongo](https://user-images.githubusercontent.com/79808404/174502569-fd5abf4b-0c96-4a11-82d6-f9f259a02858.PNG)

#### STEP 3

   I started my MongoDB server and verified that it’s running by typing the below command
      
       sudo service mongodb start

       sudo systemctl status mongodb
![sudo mongo start](https://user-images.githubusercontent.com/79808404/174502594-68c33bf3-7484-4dcb-811b-9ffbcc0a7034.PNG)

#### STEP 5
   I installed node package manager (NPM) by running
      
       sudo apt install -y npm
  
#### STEP 6
 I installed body-parser package to process Json files passed in request to server by running
  
     sudo npm install body-parser
  ![sudo install body-parser](https://user-images.githubusercontent.com/79808404/174502647-2ae3d1a6-b37b-4992-886b-937655aa7bc9.PNG)


#### STEP 7
  I created a folder named “Books” and cd into the folder by running
   
     mkdir Books && cd Books
   
#### STEP 8
 I initialized my npm project in the Books directory by running
   
     npm init
![npm init](https://user-images.githubusercontent.com/79808404/174502666-1561cc7a-0f65-4eff-a0b4-b6195b6cb38d.PNG)

#### STEP 9
   In the "Books" directory I added a file "server.js" by running “vi server.js” and input the below scripts
 
    var express = require('express');
    var bodyParser = require('body-parser');
    var app = express();
    app.use(express.static(__dirname + '/public'));
    app.use(bodyParser.json());
    require('./apps/routes')(app);
    app.set('port', 3300);
    app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
    });

![vi serverJs](https://user-images.githubusercontent.com/79808404/174502711-43b0df83-2ab6-48c6-a46a-65ecb427845d.PNG)


## INSTALL EXPRESS AND SET UP ROUTES TO THE SERVER

#### STEP 1

I installed Express mongoose to setup routes to my server by running
          
          sudo npm install express mongoose
![sudo install mongoose](https://user-images.githubusercontent.com/79808404/174502749-aaa66693-6d13-4ba2-a695-226cce28e8e2.PNG)

#### STEP 2
   In “Books” Directory, I created an “App” folder and changed directory into the folder by running 
 
              mkdir apps && cd apps
 

#### STEP 3 
   I created a “routes.js” file in the “App” folder and opened the file with "vi routes.js" and input the below code into it
 
     var Book = require('./models/book');
    module.exports = function(app) {
    app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if ( err ) throw err;
      res.json(result);
    });
    }); 
    app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      if ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
    });
    app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      if ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
     });
     var path = require('path');
     app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
     });
     };

![cat routes](https://user-images.githubusercontent.com/79808404/174502792-a75f9cee-ccd8-490f-b672-35619717b661.PNG)


#### STEP 4
   In the "Apps" directory, I created a "models" folder and cd into the folder and created a book.js file. I opened the file with vi books.js and type the code below into it

    var mongoose = require('mongoose');
    var dbHost = 'mongodb://localhost:27017/test';
    mongoose.connect(dbHost);
    mongoose.connection;
    mongoose.set('debug', true);
    var bookSchema = mongoose.Schema( {
    name: String,
    isbn: {type: String, index: true},
     author: String,
     pages: Number
    });
    var Book = mongoose.model('Book', bookSchema);
    module.exports = mongoose.model('Book', bookSchema);

![folder models and file bookJs](https://user-images.githubusercontent.com/79808404/174502848-28eee1d7-8e6c-4cd3-b82b-3e41bb0c83e1.PNG)



## ACCESSING THE ROUTES WITH AngularJS
### AngularJS provides a web framework for creating a dynamic views in web applications.
   #### STEP 1
  I cd back into the _"Books_" directory and created a _"Public_" folder and cd into the folder
  
     mkdir public && cd public
![folder public and file script](https://user-images.githubusercontent.com/79808404/174502971-fc22d611-0424-4a39-a4d6-2fa093d084d4.PNG)

#### STEP 2
   I created a “script.Js” file in the “Public” folder and opened the file by running “vi script.Js “ and input the code below
   
     var app = angular.module('myApp', []);
    app.controller('myCtrl', function($scope, $http) {
    $http( {
      method: 'GET',
      url: '/book'
    }).then(function successCallback(response) {
      $scope.books = response.data;
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
    $scope.del_book = function(book) {
      $http( {
        method: 'DELETE',
        url: '/book/:isbn',
        params: {'isbn': book.isbn}
      }).then(function successCallback(response) {
        console.log(response);
      }, function errorCallback(response) {
        console.log('Error: ' + response);
      });
    };
    $scope.add_book = function() {
      var body = '{ "name": "' + $scope.Name + 
      '", "isbn": "' + $scope.Isbn +
      '", "author": "' + $scope.Author + 
      '", "pages": "' + $scope.Pages + '" }';
      $http({
        method: 'POST',
        url: '/book',
        data: body
      }).then(function successCallback(response) {
        console.log(response);
      }, function errorCallback(response) {
        console.log('Error: ' + response);
      });
    };
    });
    
  ![folder public and file script](https://user-images.githubusercontent.com/79808404/174502959-53b627d2-bfa6-4de5-8717-d45eda01415a.PNG)

  ![vi scripts](https://user-images.githubusercontent.com/79808404/174502963-69b29375-4fd0-46c4-a78f-be0f12cace71.PNG)

#### STEP 2
  I created a _index.html_ file in the _Public_ directory and opened the file by running _vi index.html_ and input the code below
     
     <!doctype html>
    <html ng-app="myApp" ng-controller="myCtrl">
      <head>
        <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
        <script src="script.js"></script>
      </head>
      <body>
        <div>
          <table>
            <tr>
              <td>Name:</td>
              <td><input type="text" ng-model="Name"></td>
            </tr>
            <tr>
              <td>Isbn:</td>
              <td><input type="text" ng-model="Isbn"></td>
            </tr>
            <tr>
              <td>Author:</td>
              <td><input type="text" ng-model="Author"></td>
            </tr>
            <tr>
              <td>Pages:</td>
              <td><input type="number" ng-model="Pages"></td>
            </tr>
          </table>
          <button ng-click="add_book()">Add</button>
        </div>
        <hr>
        <div>
          <table>
            <tr>
              <th>Name</th>
              <th>Isbn</th>
              <th>Author</th>
              <th>Pages</th>

            </tr>
            <tr ng-repeat="book in books">
              <td>{{book.name}}</td>
              <td>{{book.isbn}}</td>
              <td>{{book.author}}</td>
              <td>{{book.pages}}</td>

              <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
            </tr>
          </table>
        </div>
      </body>
    </html>
    
    
![file indexHTML](https://user-images.githubusercontent.com/79808404/174503013-22be7c2c-2d07-4094-834a-d7f2a200dc4e.PNG)

![cat indexHTML](https://user-images.githubusercontent.com/79808404/174503015-35c0659b-a6f1-4016-8d60-24705c1cbd52.PNG)

#### STEP 3
  I cd back into Books directory and start my server by running the below command
      
       node server.js

![nodeServerJs](https://user-images.githubusercontent.com/79808404/174503051-756d3cf2-41c8-4b4c-a2fd-6b654c2ed370.PNG)


#### STEP 4
I opened TCP port 3300 in my AWS EC2 instance by adding a new security rule

![inbound 3300](https://user-images.githubusercontent.com/79808404/174503065-1c57f915-1684-4f73-8895-e04c37d4ce24.PNG)


#### STEP 5
 I opened my web browser and type in the below url to verify that my app is and server is working as expected.
       
         http://localhost:3300
         
  ![last ph](https://user-images.githubusercontent.com/79808404/174503081-38d395c1-1d0e-4f78-9b69-688d7e807365.PNG)



