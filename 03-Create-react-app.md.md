## Pre-requisites

* You should be familiar with programming concepts like *functions, objects, arrays* and to a lesser extent, *class*.
* You should also have a basic knowledge of JavaScript like iterations, recursion, closure.
* ES6 classes, import-export, arrow function.
* Familiarity with HTML.

# Introduction
This article will walk you through setting up your `first React app` and assumes you are familiar with text editors and command-line navigation.

> Here are some toolchains that need to get familiar with.
### Package Manager 
Package manager is more like the _backbone_ of your project. It automatically installs, upgrades, and configures in a consistent manner. Yarn is one such package manager. __NPM__ is the most popular package that we would be using in this example.  As we will be using the `Node package manager (npm)`, so you will need to have `Node` installed.
### Bundler
Another important part of the toolchain is __bundler__. It does what it sounds like. A bundler lets you write code in small modules which makes the entire process of development __smooth__ and __efficient__. __Webpack__ and __parcel__ is one of them. Parcel is a great alternative because it is _fast, minimal, and just what you need_. With webpack, you have the _responsibility of configuration_ whereas Parcel _automatically configures everything_ you need.
### Compiler
Moving on, the last part of our toolchain is the __compiler__. As most of you would be aware, compilers are crucial for making your program run at the first place. It basically converts high-level language to low-level language. __Babel__, the popular JavaScript compiler lets you incorporate the latest JavaScript features in your code. It acts as a toolchain that is mainly used to convert ECMAScript 2015+ code into a backward-compatible version of JavaScript in current and older browsers or environments. We would use the Babel compiler in this application development.

> We now begin with building our very first application in Reactjs. 

# 1. Set up the boilerplate application
It is possible to manually create a __React app__, but ___Facebook___ has created a _node module_ __create-react-app__ to generate a boilerplate version of a React application. It also provides an out-of-the-box `build script` and `development server` (to be discussed later with the flow).

We will use `npm` to install the __create-react-app__ command-line interface (CLI) globally __(-g)__.

Open up your terminal and run 
```
$ npm install -g create-react-app: 
```
Now that you have the CLI available for use, navigate to the __parent directory__ that you would like to place the __application within__. Then, run `create-react-app` with the name you would like to use for your project (just no capital letters :-) ).
```
$ create-react-app <name-of-your-app>
```
Before we run the app, let's take a look around the app structure and see what it contains.

# 2. React app structure
Change directories into the app you just created. If you list the contents of the directory including hidden files with command __`ls -la`__, you should see the following structure:

[5.webp](uploads/94281c810e3a948b673bc86859acb714/5.webp)

`create-react-app` has taken care of setting up the main structure of the application as well as a couple of developer settings. Most of what you see will not be visible to the visitor of your web app. React uses a tool called __webpack__ which transforms the directories and files here into static assets. Visitors to your site are served those static assets.

> Among these bunch of directories, there is one directory that we would like to explain further a little bit.
### public
This directory contains assets that will be served directly without additional processing by webpack. It contains  __index.html__ which provides the entry point for the web app. It also has a favicon (header icon) and a __manifest.json__.

The manifest file configures how your web app will behave if it is added to an Android user’s home screen (Android users can “shortcut” web apps and load them directly from the Android UI).

### Work inside the src directory

The first step here is to remove all preserved files from the `src` folder with the command:
```
$ rm -rf src/*
```
After that create an index.js file in the src directory with the command:
> First go inside src directory & then create this file with command
```
$ touch index.js
```
Now we start working on index.js file. Consider the following starter code in your index.js file
```js
import React from 'react';                          // line1
import ReactDOM from 'react-dom';                   // line2

class HelloUser extends React.Component {           // line3
    render() {                                      // line4
        return (                                    // line5
            <h1>Hello User!</h1>                    // line6
        )
    }
}

ReactDOM.render(                                    // line7
    <HelloUser />,                                  // line8
    document.getElementByID("root")                 // line9
);
```
* **Line 1 & 2**: Firstly import the required modules.
* **Line 3**: Create a `HelloUser` class.
  * Take care of the naming convention (Camel case starting with a capital letter).
* **Line 4**: Pre-defined `render` method to create customized interfaces.
* **Line 5**: Pre-defined `return` method to send customized interfaces from the `render` method.
* **Line 6**: Create JSX React element to be rendered. We will learn about it later.
* **Line 7**: Render a React element into the DOM in the supplied container.
  * **Syntax-Structure**: `ReactDOM.render(element, container)`
* **Line 8**: React element to be rendered finally.
* **Line 9**: The DOM location where the element to be rendered.

# 3. Starting the React app development server
As was stated, when you ran create-react-app, you just need to run `npm start` in your __app directory__ to start the __local server for the development purpose__. It should auto-open a tab in your browser that points to http://localhost:3000/ (if not, manually visit that address). Mention that, any changes to the source code will live-update here until the development server continues to serve.