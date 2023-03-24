# Our Requirement is to create full functional application:- 
<hr/>
project structure:-<hr/>
1. config <br/>
2. controller <br/>
3. model <br/>
4. views <br/>
5. public <br/>
6. routes <br/>
7. index.js <br/>
8. package.json <br/>
9. package-lock.json <br/>
<br/>
Next Step:- Add dependencies <br/>
```
{
    "dependencies":{
        "express":"latest",
    }
}
```

Adding Hot Reloading:- <br/> 
using nodemon :- <br/>
local server with autoreloading feature. <br/> <br/>

nodemon install <br/>
1. locally  <br/>
```
  npm install nodemon
  // or

  {
    "dependencies":{
        "express":"latest",
        "nodemon": "latest"
    }
  }
```
`` npm install``

<br/>
2. globally <br/>
`` npm install --global nodemon ``
<br/>
nodemon.cmd <br/>
nodemon.bat <br/> 
global path. <br/>
<br/>
npx nodemon index.js => ``server start``. <br/> 
``node nodemon index.js``
<br/>
in order to run using npm :- <br/>
locally :- <br/>
open package.json <br/>
```
{
    "scripts":{
        "test": "node test for error",
        "start": "node ./node_modules/nodemon/bin/nodemon.js index.js"
    }
}
```
<br/>
globally :- <br/>
open package.json <br/>
```
{
    "scripts":{
        "test": "node test for error",
        "start": "nodemon index.js"
    }
}
```

## index.js <br/> 
<hr/>

```
const express = require('express');
const app = express();
const port = process.env.PORT || '8080';

app.listen(port, ()=>{
    console.log(`Server Started on port ${port}`);
})

app.get("/", (req,res)=>{
    res.write("Hello world");
    res.end();
})

```

## MVC Updated Version:- 
<hr/>

```
const http= require('http');
const server = http.createServer();

const port = process.env.PORT || '8080';

server.listen(port, ()=>{
    console.log(`Server Started on port ${port}`);
})
```


## app.js 
<hr/>

```
const express = require('express');
const app = express();

module.exports = app;
```

## index.js or server.js 
<hr/>
```
const http= require('http');
const app = require('./app');

const server = http.createServer(app);

const port = process.env.PORT || '8080';

server.listen(port, ()=>{
    console.log(`Server Started on port ${port}`);
})

```

<br/>
All EndPoints, in Api and url fro get,post,put,patch,delete are managed by, routes. <br/>
<br/>
## by default Route :-
```
app.get('/xyz-url', (req,res)=>{
    res.send('get-url');
});

app.post('/xyz-url', (req,res)=>{
    res.send('post-url');
});
```

## Advance Routing :- 
<hr/>
```
express has inbuilt router.
```
## routes 
<hr/> 
<br/>
crud : studentRoute.js <br/>
    get <br/>
    post <br/>
    put <br/>
    delete <br/>
<br/>
    crud : userRoutes.js <hr/>
    get => All user <br/>
    post <br/>
    put <br/>
    delete <br/>
    login => post + get <br/>
    userVerification => user <br/>
    forgetPassword => user <br/>

```
const router = express.Router();
router.get('/xyz-url', (req,res)=>{
    res.send('get-url');
});

router.post('/xyz-url', (req,res)=>{
    res.send('post-url');
});

module.export = router;
```
<br/>
## connecting app.js with routes :- <hr/>

1. app.js connect with router. <br/>

```
const studentRoute = require('./studentRoute');
const userRoute = require('./userRoute');

app.use('/student',studentRoute);
app.use('/users',userRoute);
```

## Adding Controller to Project:- 
<hr/>

consider the code of routes <br/>
	
Before controller :-<br/><br/>
	
``` 
	router.post('/xyz-url',(req,res,next)=>{
	 res.send('post url');
	});
```	
After controller :- <br/>
```
	StudentController.js 
	//class level code 
	or 
	//function level        (next middleware or next closure can be used as callback)
	                            |
								|
	const Student = {
	getStudent: function(req,res,next){  
	  //logic
	}
	
	CreateStudent:function(req,res,next){
		//logic
	}
	
	UpdateStudent:function(req,res,next){
		//logic
	}
	
	DeleteStudent:function(req,res,next){
		//logic
	}
	
	}
	
	module.export = Student;
```

##	connect with Route with controller:- 
<hr/>
```
	const StudentController = require('./StudentController');
	
	router.get('/student-list',StudentController.getStudent);
	router.post('/register',StudentController.createStudent);
	router.put('/change-profile',StudentController.UpdateStudent);
	router.delete('/delete-account',StudentController.DeleteStudent);
```

##	control flow of Express Application  or lifeycle Node express 
<hr/>
	
```
npm ---> package.json ---> start ----> nodemon (not required in live server)---> index.js 
index.js---> config ----> app.js ----> use Middleware -----> router -----> controller
```

## Adding pages to the Node Application :-
<br/>
till now we are able to create, index,app,routes and controller in the Node Application.
<br/>
But our Node Application may require static pages to be created. <br/>
for this we can make a pages or static folder in project <br/>
<br/>
project structure :-
<br/>
```
1. config
2. controller
3. model 
4. views
5. public
6. routes
7. index.js
8. package.json
9. package-lock.json
10. pages <----------------------- This is pages folder (or static)
                | ------------> index.html
                | ------------> about.html
                | ------------> contact.html
                | ------------> register.html
                    ......
                    .....n 
                    here we can make static folder name also.
                    Note :: All the pages will be loaded in GET Request.
```
   
## How to load pages in Node Js :-
 <hr/>
 ```
 const path = require('path');

    router.get("/home",(req,res,next)=>{
        let home_page = path.join(__dirname, "./pages/home.html")
        res.sendFile(home_page)
    })
```

##  Sending 404 Page 
<br/>
<br/>
fallback : fallback when responce is ended without, expectation. <br/>
error.html <br/>
or <br/>
404.html <br/>

```
        <h1>404 page Not Found</h1>

        app.use('*',function(req,res,next){

            let pagename = path.join(__dirname, "./pages/404.html")
            res.sendFile(pagename)
        })
```

## Adding controller to the Application :-
  <br/>
controller :- It seperated Business logic from the Application. <br/>
it acts as middle man, B/w Model and View. <br/>
    Controller takes the data from model and return to view. <br/>
    and vice-versa. <br/>
<br/>
    three ways of writing the controller <br/>
    1. Normal Functions or Anonymous function to reference. <br/>
    2. Factory Function or Factory Object.(Function as Object) <br/>
    3. using classes or TypeScript. <br/>

## Normal Function as a controller :- <hr/>

```
router,get('/home',HomeController.create());
                            |
                            |
                            |
                        import/require
                            |
                            |---->package.json ---> "type":"module" or rename : .js to .mjs
                            |
                            |
                            |------> require ---> const HomeController = require('../Controller/HomeController')
                            |
                            |
                            |
                            module.exports = create;

    function create(req,res,next){
        // business logic
    }
```


## 2. Factory Function :- 
<hr/>
```
const Student :{
    create: (req,res,next)=>{
        // logic

        next()
    }
    update: (req,res,next)=>{
        // logic

        next()
    }
    delete: (req,res,next)=>{
        // logic
        
        next()
    }
}

// How to call:-
Student.create();

// How to export :-
module.exports = Student;
```

## 3. Using Class controller
<hr/>
```
class StudentController
{
    constructor(){}

    home(req, res, next)
    {
        // logic
    }

    create(req, res, next)
    {
        // logic

    }
    show(req, res, next)
    {
        // logic

    }
}

// How to export:-
module.exports = StudentController;

// How to import and use:-
const StudentController = require("../controller/StudentController.js");


router.get('/', (req, res, next) =>
{
    new StudentController().home(req, res, next);
})
router.get('/create', (req, res, next) =>
{
    new StudentController().create(req, res, next);
})
router.get('/show', (req, res, next) =>
{
    new StudentController().show(req, res, next)
})

// other ways
// ------------

const student = new StudentController();

router.get('/', (req, res, next) =>
{
    student.home(req, res, next);
})
router.get('/create', (req, res, next) =>
{
    student.create(req, res, next);
})
router.get('/show', (req, res, next) =>
{
    student.show(req, res, next)
})

```



### Thanks for reading. This notes written by prabhat.
