To share a state between two components, the most common operation is to move it(state) up to their closest common ancestor(parent component). This is called __“lifting state up”__. (i.e. removing the local state from the descendant(child) component and move it into its ancestor(parent) instead.)

__Shared State:__ When we update an input, other components should reflect the change (and vice versa).

The descendant(child) components hence become “controlled” in order with the parent component.

It has been quoted from the official docs about __Lifting State Up__ as, 
> ___`Often, several components need to reflect the same changing data. We recommend lifting the shared state up to their closest common ancestor.`___

Let's understand this with a simple example.
```js
function Counter(props) {
    const [counter, setCounter] = useState(0);

    function decrement() {
        setCounter(counter => counter - 1);
    };

    function increment() {
        setCounter(counter => counter + 1);
    };

    return (
        <div>
            <button type="button" onClick={decrement}>-</button>
            {counter}
            <button type="button" onClick={increment}>+</button>
        </div>
    );
}
function App () {
    return(
        <div>
            <Counter />
            <p>You have clicked 0 times!</p>
        </div>
    )
}
```
First off, we have a `Counter` component. This component has the state initialized with a value of counter set to `zero`. Also, we have `increment` and `decrement` methods.

Now, we want "You clicked 0 times" to increment on clicking the button. In order to do that, could we add `clicks: 0` field in `Counter` like this and pass this data in the state to the App component? It's quite difficuilt to see any other solution till now.
```js
function Counter(props) {
    const [counter, setCounter] = useState(0);
    const [clicks, setClicks] = useState(0);
    ...
}
function App () {
    return (
        <div>
            <Counter />
            <p>You have clicked 0 times!</p> // how do I update this ?
        </div>
    );
}
```
Well, that's not possible with this set of codes.

So, the solution would be __lift the state__ up. Instead of keeping the state in `Counter`, we move the state to our App component. See here.
```js
function Counter(props) {
    const { increment, counter, decrement } = props;

    return (
        <div>
            <button type="button" onClick={decrement}>
                -
            </button>
            {counter}
            <button type="button" onClick={increment}>
                +
            </button>
        </div>
    );
}

function App() {
    const [counter, setCounter] = useState(0);
    const [clicks, setClicks] = useState(0);
    function decrement() {
        setCounter(counter => counter - 1);
        setClicks(clicks => clicks + 1);
    };

    function increment() {
        setCounter(counter => counter + 1);
        setClicks(clicks => clicks + 1);
    };
    return (
        <div>
            <Counter 
                increment={increment}
                decrement={decrement}
                counter={counter} 
            />
            <p>You have clicked {clicks} times!</p>
        </div>
    )
}
```
Now, the funny part is, you might've done this unknowingly before or you'd consider this approach to be the general way of doing it. We totally agree with you, but yeah, that's the whole __lifting state up__ strategy is about.