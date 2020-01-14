---
layout: post
title:      "React Hooks"
date:       2020-01-07 16:43:19 +0000
permalink:  react_hooks
---


When going through the Flatiron School curriculum Hooks were only mentioned once and it was just a link to the video of the announcement of Hooks. I heard from several of my friends that I really needed to learn and understand Hooks, so after I graduated I did. Hooks are great!

What are React Hooks? The React documentation defines a Hook as a 'special function that lets you "hook into" React features.' The two that I find myself using regularly are useState() and useEffect(). So let's dive into each one a little. 

#### UseState()
First, lets look at an example of using state before Hooks:
```
class Book extends Component {
    state = {
	  title: "The Cat in The Hat"
	}
	
	handleTitleChange = event => {
	    this.setState({
		  title: event.target.value
		})
	}
	
	render() {
	    return (
		    <form>
			  <label>Title:</label>
			  <input type="text" value={this.state.title} onChange={this.handleTitleChange}/>
			</form>
		)
	}
	
}
```
So, we use a class component since we need a state, and we initialize the state with an object that has the key 'title'. The input value is set to the state.title and when the user changes that input handleTitleChange() is called. handleTitleChange() changes state.title by calling setState() and setting it to the value of the input control. This should all look pretty familiar and normal if you have done anything at all with React. So, now let's look at an example with hooks:
```
const Book = () => {
    const [title, setTitle] = useState("The Cat in The Hat");
		
	const handleTitleChange = event => {
	    setTitle(event.target.value);
	}
	
	render() {
	  return (
		  <form>
			<label>Title:</label>
			<input type="text" value={title} onChange{handleTitleChange}/>
		  </form>
	  )
    }
}
```
The first thing you might notice here is that Book is no longer a class component. This is probably my favorite thing about using Hooks. If I already have a functional component that I find needs a state, I do not have to rewrite the component to make it a class component. Instead I just create a state variable at the top with useState(). When using useState() we get access to a state variable and a function that allows us to change the state variable. In the example above the state variable is 'title' and the function to change 'title' is 'setTitle'. This simplifies changing state greatly. Just look at how much shorter handleTitleChange() is! Reading state is also simplified. You can see that in our input tag we can just call 'title' instead of 'this.state.title'.

#### useEffect()
Next up is useEffect(). useEffect() essentially combines componentDidMount(), componentDidUpdate(), and componentWillUnmount(). Let's look at an example before useEffect() building off of the previous examples:
```
class Book extends Component {
    state = {
	  title: "The Cat in The Hat"
	}
	
	componentDidMount() {
	  document.title = this.state.title;
	}
	
	componentDidUpdate() {
	  document.title = this.state.title;
	}
	
	handleTitleChange = event => {
	    this.setState({
		  title: event.target.value
		})
	}
	
	render() {
	    return (
		    <form>
			  <label>Title:</label>
			  <input type="text" value={this.state.title} onChange={this.handleTitleChange}/>
			</form>
		)
	}
	
}
```
We added two functions to our compononent. So now when the component mounts and when the component updates the document.title will be updated to equal state.title. Notice the repetition here! So, now let's look at this component using the useEffect() hook:
```
const Book = () => {
    const [title, setTitle] = useState("The Cat in The Hat");
	
	useEffect(() => {
	  document.title = title;
	});
	
	const handleTitleChange = event => {
	    setTitle(event.target.value);
	}
	
	render() {
	  return (
		  <form>
			<label>Title:</label>
			<input type="text" value={title} onChange{handleTitleChange}/>
		  </form>
	  )
    }
}
```
That looks much better. Now, using useEffect() we don't have to have repetitious lines of code. Any time the component renders or rerenders React remembers the function that you passed into the useEffect() Hook and calls it. By default useEffect() runs after every render, but we can control it a bit. useEffect() accepts a second optional argument. If the variable passed in does not change during the rerender then the function is not called. Example:
```
useEffect(() => {
  document.title = title;
}, [title]);
```
Now, useEffect() will only call the passed in function if 'title' changes during the rerender. Another way to use this second argument is to simply pass an empty array [] when you only want the passed in function called when the component mounts and not when the component updates. 

React Hooks have been pretty easy to pick up and implement for the most part. There is a ton of documentation and examples on ways to implement and use them, and they have made coding components much better for me. I no longer have to think about whether a component should be a class component or a functional component. I just start writing and add state if I need. As you can see from above, using Hooks also shortens your code which is always nice. If you want to find out more about React Hooks and how to use them a good starting place is here: https://reactjs.org/docs/hooks-intro.html
