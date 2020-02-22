---
layout: post
title:      "Tests First"
date:       2020-02-22 17:57:58 +0000
permalink:  tests_first
---


One thing that I have started doing now, that has been very helpful in all of my coding projects, is writing my tests first.

Flatiron briefly went over writing tests and the whole course was based on Test Driven Development, because each assignment was based on passing tests. While going through the course though, I admittedly skimmed through the lessons of writing tests because I was more concerned with learning the other topics of the course and building out my projects. 

But then I started interviewing. Sometimes companies will say "Write a functions called X that takes in Y as an argument and returns Z." and they will provide sample inputs and outputs. If you write the tests beforehand, you cannot fail! I thought learning to write small tests would take a long time, but the basics are incredibly simple, so let's write one!

## Assignment
Let's say we are told to write a function called 'isPalindrome()' that takes in a string as an argument and if the argument is a palindrome then it will return the string 'true', and if the argument is NOT a palindrome then it will return the string 'false'.

### Basics
Before we write the tests we need a couple tools to help us out. First, we can install [Mocha](https://mochajs.org/). Mocha is the testing framework that will handle running our tests. The second tool to install is [Chai](https://www.chaijs.com/) which is an assertion library. With those installed we can start writing our tests in a file called test.js. Let's first import the 'expect' module from chai and our function from isPalindrome.js.

```
// test.js

let expect = require("chai").expect;
let isPalindrome = require("../app/isPalindrome");
```

### The tests
Now, that those are imported, let's write the actual tests. The first function we need to learn is describe(). describe() is a function that allows you to, you guessed it, describe a group of tests. You can also nest describe() functions if you want to further divide things up and be very descriptive. describe() takes in two arguments. The first is a string that describes the test group. The second is a callback function. So, let's describe our tests with describe():
```
// test.js

let expect = require("chai".expect;
let isPalindrome = require("../app/isPalindrome");

describe("Palindrome Test", function() {

});
```
Cool, we have a group here that we can see will test for palindromes. The next function that we need to learn is it(). it() is used for individual test cases and is helpful if written as if you were saying it out loud. it() also takes in two arguments just like describe. The first is a string that describes what the test should do. The second is a callback function. Let's add a couple it() functions to test for both cases of our function:

```
// test.js

let expect = require("chai".expect;
let isPalindrome = require("../app/isPalindrome");

describe("Palindrome Test", function() {
  it("returns the string 'true' if the word is a Palindrome", function(){
	
  });
	
  it("returns the string 'false' if the word is NOT a Palindrome", function(){
	
  });
});
```
The callback function that gets passed in as the second argument of it() is where all the meat is. Let's write it out and then explain.

```
// test.js

let expect = require("chai".expect;
let isPalindrome = require("../app/isPalindrome");

describe("Palindrome Test", function() {
  it("returns the string 'true' if the word is a Palindrome", function(){
    let test1 = isPalindrome("KAYAK");
    let test2 = isPalindrome("level");
    let test3 = isPalindrome("RaCecar");
		
    expect(test1).to.equal("true");
    expect(test2).to.equal("true");   
    expect(test3).to.equal("true");
  });
	
  it("returns the string 'false' if the word is NOT a Palindrome", function(){
    let test1 = isPalindrome("hello");
    let test2 = isPalindrome("RaCeDog");
    let test3 = isPalindrome("GOODBYE");
		
    expect(test1).to.equal("false");
    expect(test2).to.equal("false");   
    expect(test3).to.equal("false");
  });
});
```
The way that this is written should be very easy to understand by just reading it out loud. The variable 'test1' is set to the return value of 'isPalindrome("KAYAK")'. At that point we 'expect test1 to equal "true"'. It is that easy!

## Notes
Now of course there is more to running the tests then just writing them. You will have to define the test directory in your package.json file. After that if you have Mocha installed globally then you should be able to just type 'mocha' in the terminal and run the tests with all defaults. If you wanted to use a specific test reporter or some other tools like nyc then the best way is to define your test script in the package.json file and run your tests that way.

Writing the tests before you write your code is a way to guarantee that your code does what is required, and it makes your tests and code less opinionated. Since I have graduated from Flatiron I have been trying to write my tests before any of my code now, and I find that it also allows me to write code a little quicker because I know exactly what I am trying to accomplish. 


