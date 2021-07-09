## What are the components?
Components are the building blocks of any React app and a typical React app will have many of these. Simply put, a component is a JavaScript __class__ or __function__ that optionally accepts inputs i.e. properties(props), and returns a React element that describes how a section of the UI (User Interface) should appear.
#### Your first component
```js
function Greeting() {
  return (
    <div>
      <h1>Hello user.</h1> 
      <p>Great to see you after so long.</p>
    </div>
  );
}
```
When the component is created as a JS function, it is referred to as a ___functional component___. Here __Greeting__ functional component is actually a composition of elements: _div_, _h1_, and _p_.

A React component is rendered in the same way as an element, by using the render() method of the ReactDOM. However, instead of calling the function name, as we do in JS, we call the React component.

__So we don’t write:__
```js
ReactDOM.render(Greeting(), document.getElementById('id'));
```
__Instead, we write:__
```js
ReactDOM.render(<Greeting />, document.getElementById('id'));
```
#### A different way to write components
So far, you’ve written a functional component, a fitting name since it really was just a function. Components can also be written using ES6 classes instead of functions. Such components are called ___class components___. So, the above __Greeting__ functional component can be written in form of class component as:
```js
class Greeting extends React.Component {
  render(){
    return (
      <div>
        <h1>Hello user.</h1> 
        <p>Great to see you after so long.</p>
      </div>
    );
  }
}
```
## What are props and who uses them?
When you want to pass some attributes to an HTML element, you do it like this:
```js
// The text "Title attribute" will be displayed as a tooltip by the browser
<p title="Title attribute"> Title </p>
```
In a JS function, when you want to pass parameters as input, you use this syntax:
```js
function fctName(parameter) {
  return parameter;
}
```
When we call this function, the __values__ that we pass as parameters are referred to as __arguments__. Inside the function, the parameters behave like local variables. In React, __props__ are _properties_ that serve as __arguments__ or input for React components, in the same way that parameters passed to a regular JS function serve as arguments.
```js
// React functional component with props
function Greeting(props) {
  return (
    <div>
      <h1>Hello {props.name}.</h1> 
      <p>Great to see you after {props.time}.</p>
    </div>
  );
}
```
When we render this component, we can pass it a specific _value_ for the _name property_. This value will be passed as an argument, in the same way that we pass attributes to HTML elements:
```js
const el = <Greeting name="Saki" time="3 years" />;
ReactDOM.render(el, document.getElementById('id'));
```
So this is how the code looks like:
```js
function Greeting(props) {  
   return (
    <div>
      <h1>Hello {props.name}.</h1> 
      <p>Great to see you after {props.time}.</p>
    </div>
  );
}
const el = <Greeting name="Saki" time="3 years" />;
ReactDOM.render(el, document.getElementById('id'));
```
__props is an object__, so in the code above, when you pass props, you’re actually passing {name: “value”}. To access the name, it’s now obvious that you need to write props.name.
### How props will be treated in the class components?
Here the differences comes during interaction with props is the way to __access it__ inside component and the __constructor setup__. Let's have a look at the same Greeting component:
```js
class Greeting extends React.Component {
  render(){
    return (
      <div>
        <h1>Hello {this.props.name}</h1> 
        <p>Great to see you after {this.props.time}</p>
      </div>
    );
  }
}
const el = <Greeting name="Saki" time="3 years" />;
ReactDOM.render(el, document.getElementById('id'));
```
## Composing Components
__Compose__ means _to create_ or _to frame_. Now the question comes into your mind would be that ___how could we build larger complex components?___ So the answer is very simple __by reusing smaller components in combination__.

Let's consider a situation of framing a __Quiz__ ___navbar___ consisting of a `title` alongwith `sub-title` below it on the leftside of the __navbar__ and a `submit` __button__ on the rightside of the __navbar__ to __submit the Quiz__.
![comp1](uploads/3180d21d9f4c2b3b671bc649512c90a2/comp1.png)