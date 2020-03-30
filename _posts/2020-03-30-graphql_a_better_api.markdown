---
layout: post
title:      "GraphQL: A Better API"
date:       2020-03-30 15:37:56 +0000
permalink:  graphql_a_better_api
---


After I finished Flatiron School, I thought the only way to implement an API was to implement a RESTful API. I was wrong. I knew there were some older less used APIs like SOAP, but I had no idea there was anything that was newer. Then I started working on a community project, and when I looked at the server code I had no idea what was going on. The reason was because the project, like a lot of new projects was using GraphQL.

## Introduction
GraphQL is "a query language for your API". GraphQL is a lot more flexible than REST and instead of defining all of the objects on the backend like you do with REST, you can define your object on the frontend and get exactly the information you need.

## Types
When implementing a GraphQL service you will define several types on the backend, such as Users, Books, Movies, etc. A simple User type might look something like this:
```
type User {
  id: ID!
  name: String!
  email: String!
  password: String!
  age: Int
  bio: String
  books: [Book!]!
}
```
Here we have a User that has fields id, name, email, password, age, bio, and books. Each field has a scalar type that defines what kind of information it will return when queried. For instance if you query the age field then you will get an Integer back. If the scalar type is followed by a '!' like 'name: String!' then it means that the name field is non-nullable. And when we define a field like 'books: [Book!]!' then when you query books you will get an array of Book objects. Since the array is non-nullable then you will always be returned an array, even if it is empty, and since Book! is non-nullable then every element in the array will be a Book object.

There are two special types that we will talk about next: Query and Mutation

## Queries
Query and Mutation types define an entry point into the GraphQL service. Let's say we wanted to get a single User object by sending in an id. We would define the query like this:
```
type Query {
  user(id: ID!): User
}
```
This defines the query 'user' that accepts and id as an argument and returns a User object. With that defined we could query the service like this:
```
query {
  user (
    id: "1"
  ) {
    name
    email
    age
    bio
  }
}
```
In this query we pass in the argument of '1' for the id and then this is where the magic of GraphQL comes in. Instead of getting back all parts of the User object, we specify what fields we want to receive. In this case we specifed that we want the name, email, age, and bio field. Awesome!

Now, that is not all that you have to do. We will not go over it here, but in order to actually use the query you have to also define how the service handles that query by implementing a resolver.

## Mutations
Mutations are like queries except they modify data on the server side, and while technically you could define everything as a query it is convention to separate them out so you know exactly what you are working with and when data writes will happen. 

Let's say we want to define a mutation to create a user. It might be defined like this:
```
type Mutation {
  createUser(name: String!, email: String!, password: String!, age: Int, bio: String): User
}
```
We defined a mutation called 'createUser' that accepts arguments for all of the possible fields for a User object, and it will return a User object, which we can infer will be the new User. We would implement that in the resolver so we know the new User is the one that will be returned. Again, note the '!' following the scalar types indicating that these are required. With 'createUser' defined we can use it like this:
```
mutation {
  createUser(
    name: "Bill"
    email: "bill@aol.com"
    password: "supersecret"
    age: 36
    bio: "Computer Nerd"
  ) {
    id
  }
}
```
Here, we called the createUser mutation with the arguments name, email, password, age, and bio. Then we tell the server what information we want returned from the new User object that is created, the id. 

## Conclusion
GraphQL is really flexible and allows you to define the data you want on the frontend instead of getting back everything about the object and having to sift through the data. The learning curve with GraphQL is definitely steeper than learning how to implement a RESTful API, but they have great tutorials for implementing it with various tech stacks. More can be found here:

[GraphQL](https://graphql.org/)

[Tutorials](https://www.howtographql.com/)

