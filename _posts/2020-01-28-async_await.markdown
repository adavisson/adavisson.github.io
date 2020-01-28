---
layout: post
title:      "async/await"
date:       2020-01-28 19:17:01 +0000
permalink:  async_await
---


When going through the Flatiron curriculum it took me a little while to grasp how Promises work, and then also how to use them. Once I understood exactly what a Promise was (*an object representing eventual completion of an asynchronous operation*) things got a lot better, but chaining asynchrounous operations with .then() calls seemed very clunky and making things flow was not intuitive.

In steps async/await. Let's look at an example using .then() calls:

```
fetch('https://api.collegefootballdata.com/teams/fbs')
  .then(resp => resp.json())
  .then(data => {
    someFunction(data)
  })
  .catch(error => {
	console.log("Error: " + error.message);
  });
```
Now, let's look at this same operation using async/await:
```
const fetchData = async() => {
  let resp = await fetch('https://api.collegefootballdata.com/teams/fbs');
  let data = await resp.json();
  someFunction(data);
}

fetchData();
```
That looks much cleaner and easier to read! So, what is happening here? Let's define async and await:

#### async
async: a keyword that you put in front of a function to turn it into an asynchronous function.

This does a couple important things. First, it allows us to use the *await* keyword because the function now knows to look for it. Second, is now that is an asynchronous function, it returns a Promise. Since it returns a promise we could also combine the two methods, which can allow you to be more flexible and do something like this:

```
const fetchData = async() => {
  let resp = await fetch('https://api.collegefootballdata.com/teams/fbs');
  return await resp.json();
}

fetchData().then(data => someFunction(data));
```

#### await
await: a keyword that tells JavaScript to pause until the asynchronous function has returned a result. 

**The *await* keyword only works inside an async function.** All you need to do to use the *await* keyword is put it before the function call and assign the result to a variable. So, looking at this line:
```
 let resp = await fetch('https://api.collegefootballdata.com/teams/fbs');
```
JavaScript sees the *await* keyword and pauses until the fetch operation returns a result. When it receives the result it assigns it to the variable 'resp' and continues on.


#### Error Handling
In the very first code snippet above we had a bit of code that would fire if an error was thrown. We can also do that with async/await by utilizing a try/catch block. It looks like this:

```
const fetchData = async() => {
  try {
    let resp = await fetch('https://api.collegefootballdata.com/teams/fbs');
    let data = await resp.json();
    someFunction(data);
  } catch(error) {
    console.log("Error: " + error.message);
  }
}

fetchData();
```

When going through the Flatiron curriculum, we did not go over async/await much and that was fine for learning how Promises worked. It really would have been nice though for learning to control async function calls better though. Once I learned and understood how async/await works I went back through all of my projects and refactored them to use these keywords. It really makes it so much easier to read and follow what is going on.

More about async/await can be found here: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await
