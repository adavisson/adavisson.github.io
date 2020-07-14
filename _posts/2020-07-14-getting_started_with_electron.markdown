---
layout: post
title:      "Creating My Dream Email Client with Electron"
date:       2020-07-14 16:50:58 -0400
permalink:  getting_started_with_electron
---


## The Problem
I like using the Gmail web interface more than any desktop email client, and I also like not having a copy of all of my emails on my hard drive which happens with most desktop email clients. BUT, I hate having Gmail just being another tab in my Chrome window. It drives me crazy, and because of that I have experimented with many desktop clients, but none of them look, feel, or operate as good as the web interface.

## The Solution
I have always wanted to work with Electron and what better way to start out than building a simple application that presents a webpage. So that is what I did. I built a small desktop application that just shows me the Gmail web interface.

## The Process
I had thought that working with Electron would be pretty straight forward... and I was right. Getting started was incredibly simple. Following the guide [here](https://www.electronjs.org/docs/tutorial/first-app), I was able to get an application up and running very quickly that presents a simple html page.

Let's take a look at some of the important bits. After running ```npm init```, installing electron with ```npm install electron --save-dev```, and configuring our package.json file we want to start working in our main.js file. First, we need to get a couple of important things from electron:

```
// main.js

const { app, BrowserWindow } = require('electron')
```

'app' is available to us to help us manage the lifecycle of our application and 'BrowserWindow' is what we will use to create new windows in our application. Moving on:

```
// main.js

const { app, BrowserWindow } = require('electron')

const createWindow = () => {
  const win = new BrowserWindow({
	width: 1300,
	height: 900,
  })
	
  win.loadFile('index.html')
}

app.on('ready', createWindow)
```

This little bit of code is all we need to get our first application up and running, so let's examine what is happening. First, we are declaring a function that will create a window when called by using 'BrowserWindow'. We are passing some settings to it through an object to set the width and height, and we can pass a whole bunch of options here if we wanted. A list of BrowserWindow options can be found [here](https://www.electronjs.org/docs/api/browser-window). The last thing that our createWindow function does is load an html file and displays it in our window. The last line of code in our file is listening for electron to say that it is 'ready' to start creating browser windows and then calls our createWindow function.

We have an application! This is a great start, but now I want to see Gmail. After all, that is the whole reason we are here. It turns out that making this application show the Gmail webpage is as simple as changing one line of code. Take this line:
```
win.loadFile('index.html')
```
And replace it with:
```
win.loadURL('https://www.gmail.com')
```

Boom! Now, when we get a new window, instead of loading a static html page that we have, we are loading 'https://www.gmail.com'. That is all it takes.

## What Now?
Now, we add more things. That was too easy and it doesn't quite look like what I am used to seeing, because I am used to an extension in Chrome called 'Simplify Gmail' that makes my page look all neat and tidy. It turns out that adding this extension is also very easy. 

First we need to find out where this extension is installed on our computer which might be the trickiest part, but [here](https://www.electronjs.org/docs/tutorial/devtools-extension) is a tutorial on how to find it. Ignore the rest of the page, the only important part here is locating the extension on the computer. Let's add the extension:

```
// main.js

const { app, BrowserWindow, session } = require('electron')
const path = require('path')
const os = require('os')

const createWindow = () => {
  const win = new BrowserWindow({
	width: 1300,
	height: 900,
  })
	
  win.loadURL('https://www.gmail.com')
}

app.on('ready', async () => {
  createWindow()
  try {
    await session.defaultSession.loadExtension(path.join(os.homedir(), /* path to extension */))
  } catch (err) {
    console.log(err)
  }
})
```

Okay, we have loaded the extension and did a bit of refactoring. Let's step through it. First, we grabbed 'session' from electron to help us manage our browser session. Then we brought in 'path' and 'os' to help us a bit with making a file path to the extension that should work on all devices of the same platform. Once our app is ready we want to create the window and load the extension. The loadExtension function we are using returns a promise so we need to wrap it in an async function and wait for it to complete. We also want to add a bit of error handling in case the extension could not be loaded so that is why we are using a try/catch block. With that, our extension is loaded and we have a pretty Gmail web interface in our desktop application. But, wait...

## There's More!
I am using a Mac and I want this desktop application behave like a good Mac application should. If I close a window, I do not want the application to quit. So, let's fix that. At the bottom of our main.js file add this:

```
// main.js

... 

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit()
  }
})
```
This will quit the application when all of the windows close on any system that is not a Mac, but the application will remain running on a Mac. Next, if the application is still running, but there are no windows then we want a new window to launch when clicking on the icon. We can achieve that with this bit of code:

```
// main.js

...

app.on('activate', () => {
  if (BrowserWindow.getAllWindows().length === 0) {
    createWindow()
  }
})
```
Now, if there are no windows, the application is still running, and we click on the icon then the createWindow function will get called and we will get a new window. We're done!

## No we're not
We got a problem. I was using my application and when I clicked a link it tried to open a new window in my application instead of opening in Chrome. That is not what I want, so let's fix it. In our createWindow function we are going to add a little bit of code:

```
// main.js

...

const createWindow = () => {

...

  win.loadURL('https://www.gmail.com')
  win.webContents.on('new-window', (e, url) => {
    e.preventDefault()
    require('electron').shell.openExternal(url)
  }
}

...
```
What we are doing here is listening for events that want to open an new window, and instead of allowing that to happen within our app, we prevent the default action and open the url in an external application. Now, we really are done.

## Conclusion
This was a really quick and fun project that I am so glad I finally worked on, because now I have the Gmail client that I have always wanted. Electron was super easy to get started with, and there is a lot of documentation out there to help you when you hit a problem. I also packaged this application to use on my local computer with [electron-packager](https://github.com/electron/electron-packager). If you would like to view the source code for this little project then you can check it out [here](https://github.com/adavisson/gmail-electron).

