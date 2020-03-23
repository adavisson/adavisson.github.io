---
layout: post
title:      "Working With Sequelize"
date:       2020-03-23 02:11:31 +0000
permalink:  working_with_sequelize
---


In my last blog post I talked about using Express for routing in my Node.js applications. The other tool I use regularly when working with Node.js applications is Sequelize.

### What is Sequelize?
From the website: "Sequelize is a promise-based Node.js ORM for Postgres, MySQL, MariaDB, SQLite and Microsoft SQL Server." Sequelize allows you to connect your models with a relational database to provide persistence in your application.

### Initializing
To get started you need to install sequelize and sequelize-cli. After that you can run 'sequelize-cli init'. This command will create four directories: config, models, migrations, and seeders. The config directory contains a configuration file for connecting to the database. The models directory contains the model files for your application. The migrations directory contains the migration files for you application, and the seeders directory contains seeder files for you application. It is all pretty self explanatory.

### Models & Migrations
For this blog post I have two models from a simple project that I set up a few weeks ago. There is a Subject model and a Question model. A Subject has many Questions, and a Question belongs to a Subject. 

The first step is to define your models and set up your table migrations. Sequelize has a model generator that will do both of these for you in one step. Here is how you would generate the Subject model and migration:
```
sequelize-cli model:generate --name Subject --attributes name:string
```
Here is the model definition it generates:
```
/*
* models/subject.js
*/

'use strict';
module.exports = (sequelize, DataTypes) => {
  const Subject = sequelize.define('Subject', {
    name: DataTypes.STRING
  }, {});
  Subject.associate = function(models) {
    // associations can be defined here
  };
  return Subject;
};
```
The thing to note here is the 'sequelize.define'. This function takes three arguments. The first is the the name of the model, the second is the model attributes, and the third is options. We will go over the .associate() function later.

And here is the migration it generates:
```
/*
*  migrations/create-subject.js
*/

'use strict';
module.exports = {
  up: (queryInterface, Sequelize) => {
    return queryInterface.createTable('Subjects', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER
      },
      name: {
        allowNull: false,
        type: Sequelize.STRING
      },
      createdAt: {
        allowNull: false,
        type: Sequelize.DATE
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE
      }
    });
  },
  down: (queryInterface, Sequelize) => {
    return queryInterface.dropTable('Subjects');
  }
};
```
You can see that running this migration will create a table called 'Subjects' with the fields id, name, createdAt, and updatedAt. You can also see that if we want to undo this migration then we drop the table.

Here are the model and migration files for the Question model:
```
/*
* models/question.js
*/

'use strict';
module.exports = (sequelize, DataTypes) => {
  const Question = sequelize.define('Question', {
    question: DataTypes.STRING,
    answer: DataTypes.STRING,
    subjectId: DataTypes.INTEGER
  }, {});
  Question.associate = function(models) {
    // associations can be defined here
  };
  return Question;
};
```
```
/*
*  migrations/create-question.js
*/

'use strict';
module.exports = {
  up: (queryInterface, Sequelize) => {
    return queryInterface.createTable('Questions', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER
      },
      question: {
        allowNull: false,
        type: Sequelize.STRING
      },
      answer: {
        allowNull: false,
        type: Sequelize.STRING
      },
      subjectId: {
        allowNull: false,
        type: Sequelize.INTEGER
      },
      createdAt: {
        allowNull: false,
        type: Sequelize.DATE
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE
      }
    });
  },
  down: (queryInterface, Sequelize) => {
    return queryInterface.dropTable('Questions');
  }
};
```

### Associations

Now we need to add our associations. This is where the .associate() function comes into play in each model. Sequelize has four types of association methods that we can use: HasOne, BelongsTo, HasMany, and BelongsToMany. Since we have a one-to-many relationship we are going to use HasMany and BelongsTo.

Let's update the Subject model with the HasMany association:

```
/*
* models/subject.js
*/

'use strict';
module.exports = (sequelize, DataTypes) => {
  const Subject = sequelize.define('Subject', {
    name: DataTypes.STRING
  }, {});
  Subject.associate = function(models) {
    // associations can be defined here
    Subject.hasMany(models.Question);
  };
  return Subject;
};
```
If you read this line out then you can guess exactly what it is doing. We are saying that the model Subject has many Questions. The argument 'models' is actually provided for us from the generator and holds all of the models that we have defined.

Now let's add the BelongsTo association to the Question model:
```
/*
* models/question.js
*/

'use strict';
module.exports = (sequelize, DataTypes) => {
  const Question = sequelize.define('Question', {
    question: DataTypes.STRING,
    answer: DataTypes.STRING,
    subjectId: DataTypes.INTEGER
  }, {});
  Question.associate = function(models) {
    // associations can be defined here
    Question.belongsTo(models.Subject);
  };
  return Question;
};
```
Again, it is exactly as it reads. The model Question belongs to a Subject.

### Querying
The last thing I want to touch on is querying, and really I just want to highlight the .findAll() method. .findAll() returns all instances of that model, but we can also pass it some objects to customize what we get in return. If we pass it an 'attributes' object then we can specify which fields we want to see returned:

```
let questions = Question.findAll({
  attributes: ['id', 'question', 'answer', 'subjectId']
});
```
This will return only the fields id, question, answer, and subjectID and not return id, createdAt, or updatedAt. Another object we can pass is a 'where' object. This allows us to search for matching criteria:

```
let questions = Question.findAll({
  where: { subjectId: 2},
  attributes: ['id', 'question', 'answer', 'subjectId']
});
```
Here we are only getting the questions that have a subjectId value of 2.

## Conclusion
Sequelize is a really great tool and this blog barely scratches the surface of what it is capable of. While the learning curve was steeper then what I remember with Rails I find myself drawn to use it more. I plan to keep working with it, experimenting, and learning more about all of the features it offers. If you would like to learn more about Sequelize then you can find all of the documentation [here](https://sequelize.org/v5/).

