---
layout: post
title:      "The Express Route"
date:       2020-03-16 22:14:47 +0000
permalink:  the_express_route
---


When starting with Flatiron School, RESTful routing was new to me. It was something I had heard mentioned several times, but I had never learned what it was. Learning with Rails was great and I loved learning it because it made the Internet as a whole make much more sense to me.

After graduating Flatiron though, it seemed that a lot of people were looking for NodeJS experience so I thought it would be a good idea to go through and learn how to set up a backend with NodeJS. The two tools I have used the most are ExpressJS (routing) and Sequelize (ORM). I have really come to love working with Express and so I am going to talk a bit about it in this blog post.

### What is Express?
Express is a minimal web framework for NodeJS. That was easy.

### Basics
Okay, let's look at the 'Hello World' example fromt he Express website first:
```
// app.js

const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => res.send('Hello World!'))

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```
The first two lines import express and create our express application calle 'app'. The third line is specifying a variable that we are assigning the port number that we would like the application to listen on. The last line is telling the application to listen on a specific port. In this case that is port 3000.

In between those lines is the meat of our application. It is where we add our routing. Let's first look at .get() which is for, you guessed it, GET requests. The first argument is the path and the second argument is a callback function. This example callback function has two arguments, the HTTP request and the HTTP response. It sends the response of "Hello World". So when a GET request is made to the root path then the server will respond with "Hello World". 

Pretty basic, but when you start adding a bunch routes it can get messy really quick, so let's expand a bit.

### app.use()
Let's say we have a books model and an authors model. We want to divide these up into to files to make things easier. This time we are going to use express.Router() to create a Router object, add our routes to it, and then export it. Like this:
```
// routes/books.js

const express = require('express');
const router = express.Router();

router.get('/', (req, res) => {
  const allBlogs = 'allBlogs';   //Get all of the books and send them in the response
  res.send(allBlogs);
});

router.get('/:id', (req, res) => {
  const blog = 'single blog';   //Find blog with id from params
	res.send(blog);
});

modules.export = router;
```
```
// routes/authors.js

const express = require('express');
const router = express.Router();

router.get('/', (req, res) => {
  const allAuthors = 'allAuthors';   //Get all of the authors and send them in the response
  res.send(allAuthors);
});

router.get('/:id', (req, res) => {
  const author = 'single author';   //Find author with id from params
	res.send(author);
});

modules.export = router;
```
Now, we change our app.js file to look like this:
```
// app.js

const express = require('express')
const app = express()
const port = 3000
const booksRouter = require('./routes/books')
const authorsRouter = require('./routes/authors')

app.use('/books', booksRouter);
app.use('/authors', authorsRouter);

app.listen(port, () => console.log(`Example app listening on port ${port}!`))

```
app.use() mounts the specified function or set of functions at the specified path. So, what we are saying here is for the path '/books' use the routes that are defined in routes/books.js, and for the path '/authors' use the routes defined in routes/authors.js. This allows us to separate things out really easily.

### Conclusion
I really enjoy working with Express.js. In my mind I feel like I have more control over how my application behaves and it is very unopionated. It is now my go-to when I starting a backend project from scratch. More on Express can be found here: [Express](https://expressjs.com/)
