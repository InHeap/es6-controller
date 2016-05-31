es-controller
===================

Controller framework for Nodejs supporting ecmascript-6. No more hassling with code in your route. es-controller automatically set up routing based on your controllers. Typescript definition is also present for strict mode.


### Contributors

Nitin Bansal https://github.com/nitinbansal1989

## Installation

    $ npm install es-controller

## Usage
Just add the routes to router object and add the handler to the express module.

```js
const es = require("es-controller");

//Your express-app
var app = express();

/*
    Add a route to 'es-controller' Router.
*/
var router = es.router;
router.add("Default", "/{controller}/{action}/{id}", __dirname + "/Controller", defaults, false);

// Add es-controller handler to express
app.use(es.handler);

```

### Route
A Route should be added to router. It should contain a path template for matching. "controller" and "action" keyword are used for matching Class and and function name respectively. Other parameters are added to the req.params object.

A Route has following parameters :-

| Parameter  | Type | Description |
| ------------- | ------------- | ------------- |       
| Name  | string  | Name of the router  |
| Route Template  | string  | Template path of the router.  |
| Directory Path  | string | Path of the Controller directory  |
| Defaults  | Map<string, string> | Map for specifying the default parameters |  
| includeSubDir  | boolean  |  Flag for finding the coltroller classes recursively in sub directories |


### Defaults
Specifies the default request parameters.

```
var defaults = new Map();
defaults.set("controller", "Home");
defaults.set("action", "index");
defaults.set("id", "1");
```

### Controllers & Paths

The paths will be generated by a convention of naming controllers and functions.

A basic example: HomeController.js
	
```js

"use strict";
const es = require("es-controller");
/*
    Will set Controller Name as "Home"
*/
class Home extends es.Controller {
    /*
        Will be translated to get("/Home/index") (HTTP-method is extracted by first item in function name)
    */
    get_index() {
        this.res.send("Returning Get Index request");
    }
    
    /*
        Will be translated to ("/Home/index") for all methods.
        Note: specified method request will have greater priority
    */
    index() {
        this.res.send("Returning Index request for all methods");
    }
    
    /*
        Will be translated to get("/Home") when no action is found.
        Note: specified method with action request will have greater priority
    */
    get() {
        this.res.send("Get Response has been created");
    }
    
    /*
        Will be translated to post("/Home") when no action is found.
        Note: specified method with action request will have greater priority
    */
    post() {
        this.res.send("Post Response has been created");
    }
}
exports.Home = Home;

```

### Load from File
Routes can also be loaded from a json file. Use the load method of the router to specify the path to the configuration file. 

```
var router = es.router;
router.load(__dirname + "/config.json", __dirname);
```

#### config.json
Specify one Object or an Array of Object. For directory path, you can use "__dirname" global variable as "{dirname}" template in config file.

```
{
    "name": "Default",
    "template": "/{controller}/{action}",
    "dir": "{dirname}/Controller",
    "includeSubDir": false,
    "defaults": {
        "controller": "Home",
        "action": null
    }
}
```

Please provide suggestions if any.
