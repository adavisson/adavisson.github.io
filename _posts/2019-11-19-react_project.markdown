---
layout: post
title:      "React Project"
date:       2019-11-19 18:00:16 +0000
permalink:  react_project
---


It's done! I have finished my final project for the course and I can definitely say that that I am ecstatic to start applying for jobs and adventure into a new career!

My final project is titled Wacky Stories and it is an application that mimics Madlibs. The user selects a template/story and is presented with inputs to fill in words for which they are given hints (ie. noun, color, animal, etc.). After the user fills in the inputs, they are presented a story with their words used in certain spots. The user then has the option to give the story a title and save it if they like.

This application has two models, Template and Solution. A solution belongs to a template and a template can have many solutions. The backend is fairly simple for this project, but one new thing I had to figure out was storing an array of text for each instance in the model. This turned out to be pretty simple by using serialize in the models for the attributes that needed to story an array.

The frontend is built with React, which I have come to really love. Working through the React curriculum, I found myself to enjoy working with React, but all of the labs were somewhat small with not very many levels of components. This led to me feeling a little overwhelmed when starting this project. After diving in and figuring out component flow a little better, this project came together pretty quickly. The project before this was very similar, but instead of React was just plain JavaScript, and that seemed to take a lot longer and was a lot messier. I like how easy it is to separate things out with React, and how to give each component a single task.

Again, I think the biggest problem for me on this project was just the flow of components and I still have a lot to go through to feel comfortable with that. Though, I am definitely now at a better spot now than I was when I started this project. Also, I think it would do me a lot of good to continue reading about fetch calls and using thunk in React.

I am thrilled to have this project completed and I think I can safely say that learning React was my favorite part of the course so this was a great project to end on.
