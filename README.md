
Introduction to APIs and Express

# SWBAT

- Understand what package.json does
- How to create and run an ExpressJS project using yarn and nodemon
- Understand and create routes using that server
- Deploy(?) to a live server


Project: To Do App V2

# Project Objectives

Use an Express server to create API endpoints related to creating a To Do App. We'll iterate on this project, so we'll add things like error handling as we go on.

# Before we begin

- [ ]  Do you have the Heroku CLI installed? (You should have if you worked with Laz or Leo before the cohort began.)
- [ ]  Do you have `yarn` installed? (Same.)

# Design the API

Up until now we've been building a front-end to handle To Do lists. Remember this? 

[wyn-todo-v01 on Glitch](https://glitch.com/edit/#!/wyn-todo-v01)

Now we're going to build some API endpoints; If you're building an API that other people will use, it helps to design the API first. Hopefully I'll achieve the following:

- I've mapped out the things I want my APIs to do.
- I've designed the API endpoints and the right HTTP requests. There is, generally, a best practice to naming these endpoints. The more you use best practices, the better.
- Showing examples of request and response bodies.

# Let's create a `[README.md](http://readme.md)` that illustrates all of this.

$ mkdir todo-v2-c37
$ cd todo-v2-c37
$ touch README.md

# Get all items

GET `/api/items/`

Sample response body:

```json
[
    {
        "id": 1,
        "item": "get some eggs",
        "completed": true
    },
    {
        "id": 2,
        "item": "get some beer",
        "completed": false
    },
]
```


# Post an item

POST `/api/items/`

Sample request body:

```json
{
    "item": "the name of the thing we're including"
}
```

Once it does that, it should *return* the full object of the item (why, you think?)

```json
{
	"id": 3,
	"item": "the name of the thing we're launching",
	"completed": false
}
```

# Edit a task / mark a task as done

PUT `/api/items/:id`

```json
{
	"id": 2,
	"item": "get some beer",
	"completed": true
}
```

Returns the updated item.

# Delete a task

DELETE `/api/item/:id`

Returns the item to delete.

## Create the Express server

```bash
# the yes flag bypasses the questions
$ yarn init --yes
$ yarn add express
$ yarn add nodemon --dev
```

Add the following dev script to run nodemon:

```json
{
  "name": "todo-v2-c37",
  "version": "1.0.0",
  "main": "server.js",
  "license": "MIT",
  "scripts": {
    "dev": "nodemon ./server.js"
  },
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "nodemon": "^2.0.3"
  }
}
```

Open server.js and populate it with template:

```jsx
// server.js

/**
 * Required External Modules
 */

/**
 * App Variables
 */

/**
 *  App Configuration
 */

/**
 * Routes Definitions
 */

/**
 * Server Activation
 */
```

Now under the Required External Modules section, import the express and path pacakges:

```jsx
// server.js

/**
 * Required External Modules
 */

const express = require("express");
const path = require("path");
```

The path module provides you with utilities for working with file and directory paths. You'll use it later on to create cross-platform file paths to access view templates and static assets, such as stylesheets and images.

Next, under the App Variables section, add the following:

```jsx
// server.js

/**
 * App Variables
 */

const app = express();
const port = process.env.PORT || "8000";
```

And now we can create a simple route handler:

```jsx
// server.js

/**
 * Routes Definitions
 */

app.get("/", (req, res) => {
  res.status(200).send("TO DO App");
});
```

Finally, we can start a server listening for incoming requests on port and display a message to confirm it's listening:

```jsx
/**
 * Server Activation
 */

app.listen(port, () => {
  console.log(`Listening to requests on http://localhost:${port}`);
});
```

To integrate stylesheets and images into the application, Express needs to be configured to handle JSON requests and serve static files from a project directory.

```jsx
// server.js

/**
 *  App Configuration
 */

app.use(express.json());
app.use(express.static(path.join(__dirname, "public")));
```

Use this as a checkpoint to test if your server is working correctly.

## Run it locally

```bash
$ yarn run dev
```

## Deploy

Wait, you say, already? Yes, *already*. Ship early, ship often. We'll use Heroku's free tier to deploy our web server to a URL accessible to the internet.

[Deploying Node.js Apps on Heroku](https://devcenter.heroku.com/articles/deploying-nodejs)

```bash
$ node --version
```

Modify your package.json to reflect this version of node — this will be needed to tell Heroku it has to install this version to its servers. You'll also want to add `"start": "node ./server.js"` to your script, as heroku needs this. 

```json
{
  "name": "todo-v2-c37",
  "version": "1.0.0",
  "main": "server.js",
  "license": "MIT",
  "scripts": {
    "dev": "nodemon ./server.js",
		"start": "node ./server.js"
  },
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "nodemon": "^2.0.3"
  },
  "engines": {
    "node": "12.x"
  }
}
```

# Initialize your GitHub repo

```bash
$ git init
$ echo "node_modules/" > .gitignore
$ git add .
$ git commit -m "Initing"
$ git remote add origin [your-github-repo]
$ git push -u origin master
```

Press `heroku create` to create an app on heroku. This should generate a randomized app name for you, and add the heroku remote. You can run `heroku local web` in your command line to test first.

```bash
$ heroku create
Creating app... done, ⬢ enigmatic-castle-11202
https://enigmatic-castle-11202.herokuapp.com/ | https://git.heroku.com/enigmatic-castle-11202.git

$ git remote -v
heroku	https://git.heroku.com/enigmatic-castle-11202.git (fetch)
heroku	https://git.heroku.com/enigmatic-castle-11202.git (push)
origin	git@github.com:wyncode/todo-v2-c37.git (fetch)
origin	git@github.com:wyncode/todo-v2-c37.git (push)

$ git push heroku master

# To open the URL to a new browser
$ heroku open
```

## Build your routes

```jsx
// server.js

/**
 * Routes Definitions
 */

app.get("/", (request, response) => {
  response.status(200).send("TO DO App");
});

app.get("/api/items", (req, res, next) => {
	// TO DO
}

app.post("/api/items", (request, response, next) => {
	// TO DO
}

app.put("/api/items/:id", (req, res, next) => {
	// TO DO
}

app.delete("/api/items/:id", (req, res, next) => {
	// TO DO
}
```

Where have we seen the get, post, put and delete methods before? What do you think we're doing here?

We'll use `next` a lot more when we start implementing that error handling I was telling you about. The idea will be that if an error is thrown, next will by pass all of the other routes to trigger an error handler. 

- **Is this where we store our To Do items? I thought we need a database for that.**

### What's our battle plan for the routes?

All routes pass in `request` and `response` objects as parameters.

Requests using GET are pretty straightforward. Everything else requires something from the request header: POST requests need information from `request.body`. PUT and DELETE requests pass in information through the URL — specifically, `request.params`. 

Once we get that, we'll want to sent a response back, in JSON format: `response.json`.

- In English, what's our strategy for if we need to **get the items**?
- In English, what's our strategy for if we need to **add a new item**, given what inputs and outputs we wrote about in our API?
- In English, what's our strategy for if we need to **make a change to an item**, given what inputs and outputs we wrote about in our API?
- In English, what's our strategy for if we need to **delete an item**, given what inputs and outputs we wrote about in our API?

### Testing POST requests in Postman

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f3bfbd77-fec6-4504-b574-1d42aca1db53/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f3bfbd77-fec6-4504-b574-1d42aca1db53/Untitled.png)

# References

[Create a Simple and Stylish Node Express App](https://auth0.com/blog/create-a-simple-and-stylish-node-express-app/)

[gothinkster/realworld](https://github.com/gothinkster/realworld/blob/master/api/README.md)
