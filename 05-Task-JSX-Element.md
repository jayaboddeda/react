Since you have already __set-up-a-react-project-with-parcel__. So in this task, you have to continue from that point. As you know in the **src/index.js** file you have defined `HelloMessage` (__A user-defined class component__) which contains a _nested JSX element_ comprising of:
- `div`
  - `header`
  - `div`
    - `h1`

## So, proceeding to the task you have to:
- Define a global variable __{company}__ outside the class-component definition with a string value ___`Konfinity`___.
- In the definition of `HelloMessage` class-component, within its returning JSX element declare a `p` (paragraph) element just after the heading saying `<h1>Hello {this.props.name}</h1>`.
  - This paragraph should say __Welcome to _company___. Where __{company}__ is the global variable which you declared above with value ___`Konfinity`___.