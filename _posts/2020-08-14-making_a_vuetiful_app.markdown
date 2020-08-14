---
layout: post
title:      "Making a Vuetiful App"
date:       2020-08-14 17:33:18 +0000
permalink:  making_a_vuetiful_app
---


Lately I have been working with Vue a decent amount, and I have really grown to love how simple it is. I have also begun to work some with Vuetify which is a popular design framework for Vue, and I just want to highlight a recent project I built with Vue/Vuetify and point out some cool things.

## The Project
The project is called CFB Machine and it is hosted [here](https://cfb-machine.andydavisson.com/#/). It pulls historical college football data from this [api](https://api.collegefootballdata.com/) and presents it in a way that is fun to explore. This was a recreation of a project I made previously using React and Bootstrap. It was ugly. Really, really ugly. It was one of the first projects I felt pride in though, because my friends actually enjoyed using it, and they gave me some good feedback. A couple comparison screenshots:

Before:

![Imgur](https://i.imgur.com/Ve9tbG3l.png)
![Imgur](https://i.imgur.com/yno4jepl.png)

After:

![Imgur](https://i.imgur.com/7RSQtlVl.png)
![Imgur](https://i.imgur.com/Opb08Yzl.png)

That looks so much better. This time around I worked on putting the data into tables, having better input validation, and even added a button to switch from light mode to dark mode.

## Vue
Vue is the JavaScript framework that I used for this project. It is really simple to learn and has great documentation. Here, I just want to point out a couple of things that I really like about Vue. 

### Component Layout

```
<template>
  <h1 class="title">Hello {name}!</h1>
</template>

<script>
export default {
  name: "Hello",
  data: () => ({
    name: "Andrew'"
  }),
}
</script>

<style scoped>
  .title {
    color: orange;
  }
</style>
```

This is a basic component. Everything you need is right there. At the top is the `<template>` section, which is the html code that will be rendered. The second section is the `<script>` section which is where we put all of our imports and where we define everything about our component. Here we can define the name, data (equivalent of state in React), props, methods, etc. The third section is the `<style>` section. This is where we put all of our css related to the component. I love the convention in Vue of keeping the styling with the component. It makes more organizational sense to me.

Notice something? There is no JSX! JSX is great and easy to use once you grasp a hold of it, but you do not need to learn it to approach Vue which is pretty nice.

### Directives

Directives in Vue allow you to do some awesome things with elements like conditional rendering, iterating through objects/arrays, and binding inputs to data. Let's look at a couple easy ones:

#### v-if
```
<template>
  <h1 v-if="name != ''">Hello {name}</h1>
  <h1 v-else>Hello World</h1> 
</template>
```

Here we are telling the template that if our 'name' variable is not empty then render the h1 tag to say 'Hello, {name}'. Else, render the h1 tag to say 'Hello World'. I really like how I can look at this and know exactly what is happening, where in React I probably would have written it more like this...
```
{ name != "" && <h1>Hello {name}</h1>}
{ name === "" && <h1>Hello World</h1>}
```
which is not quite as easy to understand at first glance.

#### v-model

v-model makes controlled inputs super easy. Lets look at a simple name input first in React:
```
const [name, setName] = useState("")

return (
  <input value={name} onChange={e => setName(e.target.value)} />
);
```
We have to write a callback to update the state variable "name" every time that the input changes. Now in Vue:
```
<template>
  <input v-model="name">
</template>

<script>
export default {
  name: "HelloWorld",
  data: () => ({
    name: "",
  }),
}
</script>
```
All we had to do was use the v-model directive to associate the data variable "name" with the input and Vue handles the rest. I love v-model.

## Vuetify
Vuetify is a Material Design Framework that includes a large library of UI components that make implementing Material design incredibly simple. The documentation for Vuetify is great as well. I will not go into it much in this post because you can just read more about it [here](https://vuetifyjs.com/en/), but let's look at maybe the most important piece, theme. Here is how I defined my theme in the project:

```
new Vuetify({
  theme: {
    themes: {
      light: {
        primary: colors.lightGreen.base,
        secondary: colors.lightBlue.base
      },
      dark: {
        primary: colors.lightGreen.base,
        secondary: colors.lightBlue.base
      }
    },
    dark: true
  }
});
```
Here I defined the primary and secondary colors for the light and dark themes, and then set the default theme to dark. The theme can then be toggled like this: `this.$vuetify.theme.dark = !this.$vuetify.theme.dark;`. That was easy.

## Conclusion
Working with Vue is really simple and I enjoy how well the documentation is written. It is definitely working hard to become my frontend framework of choice.
