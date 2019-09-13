---
layout: post
title:      "Scope Methods"
date:       2019-09-13 15:44:52 +0000
permalink:  scope_methods
---


One thing that I have struggle with in both the Sinatra section and the Rails section is writing code that tries to do too much. An example from my Rails project is trying to find book objects that belong to a specific user. My solution was to retrieve ALL of the book objects, check the user_id value against the id value that I was looking for, and then adding those to an array one by one as I find them. Now, this works but it can look messy and if I had a large selection of books then retrieving all of the book objects and iterating through them could take a very long time. 

This is where scope methods come in to play. Scope methods allow us to offload the work to the database. Instead of asking the database to return all of the book objects like I did above, I could simply use a scope method to ask that the database only return book objects where the user_id value matches the desired id value. By doing this, I am only retrieving what I need, and I am allowing the database to do the work for me. It also makes the code a lot cleaner and easier to read.

I had a little trouble understanding why we would use scope methods at first, but now that I get how simple and efficient they are I am definitely going to use them wherever I possibly can.
