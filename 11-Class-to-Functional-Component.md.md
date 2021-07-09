In the previous article, you have learned how to maintain __states__ & __component's lifecycles__ in __class components__. Now it's time to do the same for __stateless components__ (__`Functional Component`__).

But before this, you must come across a question that like class components, ___how could we override the methods belongs to different lifecycles or phases `here in the functional component`___?

So here you come across one of the very delightful features of ReactJS i.e, ___`React hooks`___.
# React Hooks
- In short, _Hooks_ are a new addition in React v16.8. They let you use state and other React features without writing a class.
- Hooks `don’t` work inside classes — they let you use React without classes.
## Start with first React hook `useState`
Enter the first, and most important React hook: `useState`. It's a function exposed by react itself, you'll import it in your components as:
```js
import React, { useState } from "react";
```
> This example `renders a counter`. When you `click` the button, it `increments` the `value`:
```js
import React, { useState } from 'react';                        // line1
function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);                        // line4
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
Here, `useState` is a Hook. We call it inside a function component to add some local state to it. React will preserve this state between re-renders. `useState` returns a pair: the _current state_ value and a function that lets you update it. You can call this function from an event handler or somewhere else. It’s similar to `this.setState` in a class, except it doesn’t merge the old and new state together.

The only argument to `useState` is the initial state. In the example above, it is `0` because our counter starts from zero. Note that unlike `this.state`, the state here doesn’t have to be an object — `although it can be if you want`. The initial state argument is only used during the first render.

Let's go through the above code snippet line by line.
- __Line 1:__ We import the `useState` Hook from React. It lets us keep local state in a function component.
- __Line 4:__ Inside the `Example` component, we declare a new state variable by calling the `useState` Hook. It returns a pair of values, to which we give names. We’re calling our variable `count` because it holds the number of button clicks. We initialize it to zero by passing `0` as the only `useState` argument. The second returned item is itself a function. It lets us update the `count` so we’ll name it `setCount`.
- __Line 8:__ When the user clicks, we call `setCount` with a new value. React will then re-render the `Example` component, passing the new `count` value to it.
#### What Do Square Brackets Mean?
You might have noticed the square brackets when we declare a state variable:
```js
const [count, setCount] = useState(0);
```
The names on the left aren’t a part of the React API. You can name your own state variables:
```js
const [fruit, setFruit] = useState('banana');
```
This JavaScript syntax is called “array destructuring”. It means that we’re making two new variables fruit and setFruit, where fruit is set to the first value returned by useState, and setFruit is the second. It is equivalent to this code:
```js
var fruitStateVariable = useState('banana'); // Returns a pair
var fruit = fruitStateVariable[0]; // First item in a pair
var setFruit = fruitStateVariable[1]; // Second item in a pair
```
Using index value [0] and [1] to access them is a bit confusing because they have a specific meaning. This is why we use array destructuring instead.
#### Functional updates
If the new state is computed using the previous state, you can pass a function to `setState`. The function will receive the previous value, and return an updated value. Here’s an example of a counter component that uses both forms of `setState`:
```js
function Counter({initialCount}) {
  const [count, setCount] = useState(initialCount);
  return (
    <>
      Count: {count}
      <button onClick={() => setCount(initialCount)}>Reset</button>
      <button onClick={() => setCount(prevCount => prevCount - 1)}>-</button>
      <button onClick={() => setCount(prevCount => prevCount + 1)}>+</button>
    </>
  );
}
```
The ”+” and ”-” buttons use the functional form, because the updated value is based on the previous value. But the “Reset” button uses the normal form, because it always sets the count back to the initial value.

If your update function returns the exact same value as the current state, the subsequent rerender will be skipped completely.
#### ✌️ Rules of Hooks
Hooks are JavaScript functions, but they impose two additional rules:
- Only call Hooks __at the top level__. Don’t call Hooks inside loops, conditions, or nested functions.
- Only call Hooks from React function components. Don’t call Hooks from regular JavaScript functions. (There is just one other valid place to call Hooks — your own custom Hooks. We’ll learn about them in a moment.)
### Convert previous `Taylor Swift Quote` application to functional component
Till this point, you must be able to convert the previously designed __class-based react application__ of `Taylor Swift Quote` into __functional component-based react application__. For now, just initialize all of their required states individually and also write logics for updating states as per the event listeners. Since there is no scene of `componentDidMount`, `componentDidUpdate` and `componentWillUnmount` as it is a functional component so leave them for now. We will discuss it later.

`Note:` Before doing any kind of update to your code, make a backup of your previously designed application.

So your converted code will look something like this:
```jsx
import React, {useState} from "react";

function Li (props){
  return(
    <li>
        {props.children.quote}
    </li>
  )
}

function App (props) {
  const [time, setTime] = useState(new Date());
  const [startFetchRequest, setStartFetchRequest] = useState(false);
  const [quotes, setQuotes] = useState([]);
  function getQuotes() {
    /* method to make an api call for job data */
    const URL = `https://api.taylor.rest`;
    fetch(URL)
      .then(response => response.json())
      .then(data => {
        setQuotes(prevQuotes => prevQuotes.concat(data));
        setTime(new Date());
      });
  }
  return (
    <div>
      <h1>Hello mounting methods!</h1>
      <h2>It's {time.toLocaleTimeString()} right now</h2>
      <p>We have a collection of <span>{quotes.length}</span> quotes inspired from <span>Taylor swift</span></p>
      <button onClick={()=>{
        setStartFetchRequest(prevState => !prevState);
        console.log("You Clicked me!");
      }}>
          {startFetchRequest?"Stop":"Start"}  getting quotes
      </button>
      <div>
        <ol>
          {quotes.map((quoteObject, index)=>
            <Li key={index+quoteObject.quote}>
              {quoteObject}
            </Li>
          )}
        </ol>
      </div>
    </div>
  );
}
```
## `useEffect` React hook
You’ve likely performed data fetching, subscriptions, or manually changing the DOM from React components before. We call these operations “side effects” (or “effects” for short) because they can affect other components and can’t be done during rendering.

The Effect Hook, `useEffect`, adds the ability to perform side effects from a function component. It serves the same purpose as `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` in React classes, but unified into a single API.

By default, React runs the effects after every render — _including the first render_.

Let's refer to our above __Taylor Swift Quote__ example with `useEffect` hook, whenever __any of the state__ of `App` component gets updated, React runs the `useEffect` hook.
```jsx
import React, { useState, useEffect } from 'react';
...
function App (props) {
  const [time, setTime] = useState(new Date());
  const [startFetchRequest, setStartFetchRequest] = useState(false);
  const [quotes, setQuotes] = useState([]);
  function getQuotes() {
    /* method to make an api call for getting quote */
    const URL = `https://api.taylor.rest`;
    fetch(URL)
      .then(response => response.json())
      .then(data => {
        setQuotes(prevQuotes => prevQuotes.concat(data));
        setTime(new Date());
      });
  }
  useEffect(()=>{
    console.log("Running effect with startFetchRequest", startFetchRequest);
    console.log("Timer %s quotes %d", time.toLocaleTimeString(), quotes.length)
    let timer;
    if(startFetchRequest){
      console.log("Starting fetch service...")
      timer = setInterval(()=>{
        getQuotes();
      }, 5000);
    }
  })
...
}
```
- __What does `useEffect` do?__ By using this Hook, you tell React that your component needs to do something after render. React will remember the function you passed, and call it later after performing the DOM updates.
- __Does `useEffect` run after every render?__ Yes! By default, it runs both after the first render and after every update. Instead of thinking in terms of “mounting” and “updating”, you might find it easier to think that effects happen “after render”. React guarantees the DOM has been updated by the time it runs the effects.
- On the first render, React will call `useEffect` which will check the condition for whether the value of `startFetchRequest` is __true__ or not? which as you know will fail.
- Any kind of update in any of the state inside `App` component will lead to call `useEffect` hook.
- Now as we click on the button, it will update `startFetchRequest` from _false_ to _true_ and hence, in turn, will call `useEffect` hook.
- This, in turn, will setup a new time-interval of 5 seconds to call `getQuotes()` function which will update `quotes` and `time` state of `App` component.
- Since the state again get updates, `useEffect` hook will be called again, since this time `startFetchRequest` is __true__ so it will easily sets-up a new time-interval for __getQuotes()__ of 5 seconds while the old one was still in running mode.
- _`And this way the complete browser will hang immediately`_ or maybe the machine got crashed due to this heavy computation.
#### "clean up" the effect
The above-discussed problem can be sorted out with this "clean up" strategy given as a feature in `useEffect` hook.

Effects may also optionally specify how to “clean up” after them by returning a function. For example, this component uses an effect to set-up a timer with an interval of 5 seconds to do something (which is a function call), and cleans up by clearing this timer:
```js
...
  useEffect(()=>{
    console.log("Running effect with startFetchRequest", startFetchRequest);
    console.log("Timer %s quotes %d", time.toLocaleTimeString(), quotes.length)
    let timer
    if(startFetchRequest){
      console.log("Starting fetch service...")
      timer = setInterval(()=>{
        getQuotes();
      }, 1000);
    }
    return () => {
      console.log("Unmounting component & Stopping fetch service....", clearInterval(timer));
    }
  })
...
```
In this example, React would destroy the timer when the component unmounts, as well as before re-running the effect due to any subsequent render(because of any state update)
- Just like with `useState`, you can use more than a single effect in a component.
- __Why did we return a function from our effect?__ This is the optional cleanup mechanism for effects. Every effect may return a function that cleans up after it. This lets us keep the logic for adding and removing subscriptions close to each other. They’re part of the same effect!
- __When exactly does React clean up an effect?__ React performs the cleanup when the component unmounts. However, as we learned earlier, effects run for every render and not just once. This is why React also cleans up effects from the previous render before running the effects next time.
#### Optimizing Performance by Skipping Effects
Now, you must be thinking of the fact that during the whole `useEffect` hook execution, two states `quotes` and `time` are being updated. And the `useEffect` hook gets unnecessarily called each time due to these updates in the state which makes the application inefficient.

Because, in each hook calls, it will create a new timer with an interval of 5 sec, and before the next hook calls (as per the state updates for `quotes` and `time` once for each) it will destroy that earlier created a timer, which means the lifetime of that timer is hardly 5 seconds.

Now we should do something to skip these unnecessary effects from happening. And here this `useEffects` hook comes with one more benefit as `DependencyList`. You can tell React to skip applying an effect if certain values haven’t changed between re-renders. Or can say, it is possible to provide a comma-separated list of dependencies in Array form as a second argument to the `useEffect` hook whose update will only trigger the effect hook.

Here in our example, the effect hook of `App` should depends on `startFetchRequest` so that it gets called only when `startFetchRequest` gets updates:
```jsx
...
  useEffect(()=>{
    console.log("Running effect with startFetchRequest", startFetchRequest);
    console.log("Timer %s quotes %d", time.toLocaleTimeString(), quotes.length)
    let timer
    if(startFetchRequest){
      console.log("Starting fetch service...")
      timer = setInterval(()=>{
        getQuotes();
      }, 5000);
    }
    return () => {
      console.log("Unmounting component & Stopping fetch service....", clearInterval(timer));
    }
  },[ startFetchRequest ]);
...
```
Now, any updates in states other than `startFetchRequest` will not trigger `useEfffect` hook. If our hook requires to be triggered on `quotes` updates as well then, we have to pass `[ startFetchRequest, quotes]` as dependency list in the second argument.

`Note:` The next component you have to design is given after this complete code snippet. So, you have to go through it after getting this code-snippet and its concept correctly.
# Final code snippet
> So finally the complete code with 1 second time-interval will be like:
```jsx
import React, {useState, useEffect} from "react";
function Li (props){
  return(
    <li>
        {props.children.quote}
    </li>
  )
}
function App (props) {
  const [time, setTime] = useState(new Date());
  const [startFetchRequest, setStartFetchRequest] = useState(false);
  const [quotes, setQuotes] = useState([]);
  let timer;
  function getQuotes() {
    /* method to make an api call for job data */
    const URL = `https://api.taylor.rest`;
    fetch(URL)
      .then(response => response.json())
      .then(data => {
        setQuotes(prevQuote => prevQuote.concat(data));
        setTime(new Date());
      });
  }
  useEffect(()=>{
    console.log("Running effect with startFetchRequest", startFetchRequest);
    console.log("Timer %s quotes %d", time.toLocaleTimeString(), quotes.length) 
    let timer
    if(startFetchRequest){
      console.log("Starting fetch service...")
      timer = setInterval(()=>{
        getQuotes();
      }, 1000);
    }
    return () => {
      console.log("Unmounting component & Stopping fetch service....", clearInterval(timer));
    }
  },[ startFetchRequest ]);
  return (
    <div>
      <h1>Hello mounting methods!</h1>
      <h2>It's {time.toLocaleTimeString()} right now</h2>
      <p>We have a collection of <span>{quotes.length}</span> quotes inspired from <span>Taylor swift</span></p>
      <button onClick={()=>{
        setStartFetchRequest(prevState => !prevState);console.log("You Clicked me!");
      }}>
          {startFetchRequest?"Stop":"Start"}  getting quotes
      </button>
      <div>
        <ol>
          {quotes.map((quoteObject, index)=>
            <Li key={index+quoteObject.quote}>
              {quoteObject}
            </Li>
          )}
        </ol>
      </div>
    </div>
  );
}
```
# Hands-on experience of React hook with a small `progressbar` component
![progress-bar](uploads/a0ff4e5f03aa3e6e5e78f233e923eb8a/progress-bar.png)