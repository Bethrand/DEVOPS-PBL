# **Project 4: MEAN STACK IMPLEMENTATION**

## **TASK** — *Implement a simple Book Register web form using MEAN Stack*
### Step 1 - *Install NodeJs*

`sudo apt update` - *updates ubuntu*

`sudo apt upgrade` - *upgrades ubuntu*

#### *Add Certificates*

`sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates -`

`curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -`

##### *Install Node.js*

`sudo apt install -y nodejs`

### Step 2 - *Install MongoDB*

`sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6`

`echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list`

`sudo apt install -y mongodb` - *installs mongodb*

`sudo service mongodb start` - *starts the server*

`sudo systemctl status mongodb` - *verifies server running*

![MongoDB Server](./Images/Mongodb%20Server.png)

`sudo apt install -y npm` - *Installs NNPM Node package manager*

`npm --version` - *confirms npm version*

`sudo npm install body-parser` - *installs body-parser package*

`mkdir Books && cd Books` - *creates 'Books' directory*

`npm init` - *initializes npm project inside Books dir*

`Enter` - *hit 9 times*

`yes`

`ls` - *confirms package.js file*

`vi server.js`

`var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});`

`cat server.js` - *visualize code written correctly*

### Step 3 - *Install Express and set up routes to the server*

`sudo npm install express mongoose` - *installs express mongoose*

`mkdir apps && cd apps` - creates a directory named apps in 'Books'

`vi routes.js` - *creates routes.js file inside apps dir*

`var Book = require('./models/book');
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
};` - *routes.js file*

`mkdir models && cd models` - *creates models directory inside apps directory*

`vi book.js` - creates a file named book.js inside models directory*

`var mongoose = require('mongoose');
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
module.exports = mongoose.model('Book', bookSchema)` - *book.js file*

### Step 4 - *Access the routes with AngularJS*

`cd ../..` - *back in 'Books' directory*

`mkdir public && cd public` - *creates a public directory*

`vi script.js` - *adds a file named script.js in public directory*

`var app = angular.module('myApp', []);
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
});` - *script.js file*

`vi index.html` - *creates another file named index.html in public directory*

`<!doctype html>
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
</html>` - *index.html file*

`cd ..` - *back to Books dir*

`node server.js` - *starts the server & edit security group inbound rule to 3300*

`curl -s http://localhost:3300`

![Port 3300](./Images/Port3300%20Server.PNG)

**Book Register Web Application**

![Book Register](./Images/Book%20Register.PNG)