# Section 7 - Diving Deeper into Components & React Internals

Components are the core building block of the React library.  This section will go into more detail about components, how they work and what they can do.

**Table of Contents**     

[A Better Project Structure](#a-better-project-structure)       
[Comparing Stateless and Stateful Components](#comparing-stateless-and-stateful-components)       
[Understanding the Component Lifecycle](#understanding-the-component-lifecycle)       

## A Better Project Structure

Typically, container components like App.js and components that manage state, shouldn't be involved with UI rendering too much.  The *render()* methods in these components should be "lean" and not contain too much JSX.

If a container component is getting too big, the best approach is to spilt it into more components.

## Comparing Stateless and Stateful Components

**Stateful Component**     
```jsx
class App extends Component {
    state = {}
    // methods
    render() {
        // conditional logic
        return (
            <div className="App">
                <!-- components -->
            </div>
        )
    }
}
```
Stateful components are made using Classes and should be used as a *container* component.  They have access to *state* and *lifecycle hooks*.

**Stateless Component**    
```jsx
const myComponent = { props } => {
    // conditional statements
    return (
        <div className="MyComponent">
            <!-- Additional components/content -->
        </div>
    );
}
```

Stateless components are made using functions and don't have state.  Stateless/functional components should be used as often as possible because they are more focused and have a clear responsibility.  **They do not manage state.**

The more an application grows, the more difficult it becomes to manage state.  Using a few container/stateful components where it makes sense and placing stateless components within them will make the application easier to maintain.

| Stateful (Containers) | Stateless |
| --------------------- | --------- |
| ```class XY extends Component``` | ```const XY = {props} => {...}``` |
| ``` Access to State and Lifecycle Hooks | No Access to State or Lifecycle Hooks |
| Access State and Props via "this" | Access Props via "props" |
| this.state.XY & this.props.XY | props.XY |
| Use only if you need to manage State or access to Lifecycle Hooks | Use in all other cases | 

## Understanding the Component Lifecycle

When React creates a component, it goes through multiple lifecycle phases.  Within Stateful components, methods can be defined during these phases, which React will execute as they are encountered during the component lifecycle.

**Component Lifecycle Methods**     
* constructor()
* componentWillMount()
* componentWillReceiveProps()
* shouldComponentUpdate()
* componentWillUpdate()
* componentDidUpdate()
* componentDidCatch()
* componentDidMount()
* componentWillUnmount()
* render()

On *component creation* only the following lifecycle methods are executed:      
* constructor()
* componentWillMount()
* componentDidMount()
* render()

### constructor()

The *constructor(props)* method is a default ES6 Class.  It isn't defined by React, but React instantiates it and passes on any *props* the component receives to it.

If the constructor() method is implemented *super()* needs to be called with *props* passed in.

*State* may also be initialized inside the constructor() method.

Avoid *Side-Effects* within the constructor() method -- Side Effects are anything that could edit the state of the application (like making a request to a web server).
```jsx
class App extends Component {
    constructor() {
        super();
        this.state = {// ...};
    }
}
```

### componentWillMount()

After the constructor() method, the next method executed is *componentWillMount().  This is a method defined by React that comes from extending Component from the React library.

This method exist for history reasons mainly; it isn't used that often.  If it's used, it would be used to update State and handle last-minute optimizations.  

Like the constructor() method, componentWillMount() should not cause Side-Effects.

### render()

After componentWillMount(), the component will execute the *render()* method.  However, this does not mean that it access the DOM; instead it gives React an idea of what it *should* render if it were to reach out to the real DOM.  Whether or not it is rendered to the real DOM depends on what the real DOM looks like (if no changes need to be made, it won't be re-rendered).

In other words, the render() method prepares and structures JSX code.

### componentDidMount()

After the render() method executes and it renders any child components, *componentDidMount()* will execute, signaling that the component was successfully mounted.

At this point, Side-Effects can be introduced (make an API request, for example).  What you shouldn't do in this method is update State because it would trigger a re-render.