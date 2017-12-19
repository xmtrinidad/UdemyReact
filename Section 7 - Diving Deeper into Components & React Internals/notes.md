# Section 7 - Diving Deeper into Components & React Internals

Components are the core building block of the React library.  This section will go into more detail about components, how they work and what they can do.

**Table of Contents**     

[A Better Project Structure](#a-better-project-structure)       
[Comparing Stateless and Stateful Components](#comparing-stateless-and-stateful-components)       
[Understanding the Component Lifecycle](#understanding-the-component-lifecycle)       
[Component Updating Lifecycle Hooks](#component-updating-lifecycle-hooks)       
[Component Lifecycle Update Internal Change](#component-lifecycle-update-internal-change)       
[Performance Gains With PureComponents](#performance-gains-with-pureComponents)       
[How React Updates the App and Component Tree](#how-react-updates-the-app-and-component-tree)       
[Understanding React's DOM Updating Strategy](#understanding-reacts-dom-updating-strategy)       
[Understanding Higher Order Components](#understanding-higher-order-components)       
[Using setState Correctly](#using-setstate-correctly)       

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

## Component Updating Lifecycle Hooks

Updating lifecycle hooks has two different types: updating by the parent (changing props) and internally triggered updates (changing state).  This section covers Lifecycle Hook Methods triggered by changing props (parent).

### componentWillReceiveProps(nextProps)

This method can synchronize local state of the component to the props.  Dont cause side-effect in this method

### shouldComponentUpdated(nextProps, nextState)

*nextProps* and *nextState* refers to the upcoming props and state.

This method can cancel the updating process; if *true* is returned here the updating continues and if *false* is returned the updating stops.

Dont cause Side-Effects in this method

### componentWillUpdate(nextProps, nextState)

Sync state to props; dont cause Side-Effects

This method is better to syn state to props because if this method is executed it means it passed the shouldComponentUpdate() method and will update for sure.  Synching state to propls in any methods before could be a waste of resources because the component may not update in the end.

### render() and update child components props

Prepare and structure JSX code

### componentDidUpdate()

Here Side-Effects can be introduced, but don't update state or it will trigger a re-render.

## Component Lifecycle Update Internal Change

These are changes from *setState*

### shouldComponentUpdate(nextProps, nextState)

Updating process can be cancelled here.  Don't introduce Side Effects.

### componentWillUpdate

Sync state to props, no Side Effects

### render() and update child components

Prepare and structure JSX code

### componentDidUpdate()

## Performance Gains With PureComponents

With regular Components, executing many render() methods could be inefficient.  In many cases, render() methods will execute even if the DOM isn't updated.

One way to prevent re-rendering of child nodes is through the *shouldComponentUpdate()* method, where true or false could be returned based on a condition.  For example, if there is no change to the state, the render() methods should not be executed.

Another appraoch to checking if state or props was changed is through the use of *PureComponents*
```jsx
import React { PureComponent } from 'react';
```

The main difference between a regular Component and PureComponent is that PureComponents already have the *shouldComponentUpdate()* check already built-in.  It'll compare state/propls with their old versions and only update and execute render() methods if there was a change.

## How React Updates the App and Component Tree

Updating happens from top to bottom and only when state or props change.  With this in mind, using containers to check for updates is a best practice.  With this appraoch, containers can be monitored for changes and, if no changes occur, prevent updates to child components.

## Understanding React's DOM Updating Strategy

When the *render()* method is called it doesn't immediately render the JSX to the real DOM.  It's more like a suggestion of what the HTML should look like.  The end result of the render() method could lead to no change in the actual DOM.  For this reason, the preceding *shouldComponentUpdate()* method is used to prevent unnecessary render() calls.

What render() does is compare Virtual DOMs.  It has an Old Virtual DOM and a Re-rendered Virtual DOM.  Using this Virtual DOM approach is faster than the real DOM.

A "virtual" DOM is a DOM representation in JavaScript.  The Re-rendered Virtual DOM is the one that gets created when the render() method is called.  Again, the render() method doesn't automatically update the real DOM.  Instead, React makes a comparison between the Re-rendered Virtual DOM and the Old Virtual DOM.  If there are any differences, React will update the real DOM only in the places where changes were detected.

## Returning Adjacent Elements Using Fragments

Components usually have a wrapping element with several child elements:
```jsx
<div className={classes.Person}>
    <p onClick={props.click}>I'm {props.name} and I am {props.age} years old!!</p>
    <p>{props.children}</p>
    <input type="text" onChange={props.changed} value={props.name} />
</div>
```

Prior to React 16 multiple elements couldn't sit next to each other without the wrapping div element.  For example:
```jsx
<p onClick={props.click}>I'm {props.name} and I am {props.age} years old!!</p>
<p>{props.children}</p>
<input type="text" onChange={props.changed} value={props.name} />
```

Would not work.  There are some exceptions, such as returning an array of elements:
```jsx
const persons = (props) => props.persons.map( (person, index) => {
    return <Person 
        click={() => props.clicked(index)}
        name={person.name} 
        age={person.age}
        key={person.id}
        changed={(event) => props.changed(event, person.id)} />
});
```

The above syntax is returns an array of ```<Person />``` elements.  If an array of elements is returned, each element needs a *key* property.

With React 16.2, elements not using an array can use *Fragments* to wrap single elements without the need for a wrapping div.
```jsx
<>
    <h1>First Element</h1>
    <h2>Second Element</h2>
</>
```

Behind the scenes, Fragments are Higher Order Components that wrap the single elements.

## Understanding Higher Order Components

A HOC was used in [Section 5 - Styling React Components & Elements](https://github.com/xmtrinidad/UdemyReact/blob/master/Section%205%20-%20Styling%20React%20Components%20%26%20Elements/notes.md#using-radium-for-media-queries) when using the 3rd party package Radium.  The ```<StyleRoot></StyleRoot>``` element was used to wrap the ```<App />``` component in order for Radium to apply its styles to the application.

HOCs have a certain logic written within them.  When wrapping another component within an HOC, only that child component is affected with the logic from the HOC.  If there are multiple components that need the logic from the HOC, but several components that don't then HOCs are a good technique to use to only apply certain logic to those components that need it.

## Using setState Correctly

Besides updating state in an immutable way, there are a few other things to keep in mind regarding state.

If there is a state update that depends on the old state, there is a good way and bad way of updating it.  Take the example of updating a clicked count in state:

```js
this.state = {
    clicked: 0
}
```

A function that updates the clicked state on a button click could look like this:
```jsx
updateClicked = () => {
    this.setState({
        clicked: this.state.clicked + 1
    });
}
```

Using React dev tools, it would appear to work correctly but it isn't the right way to do it because the *setState()* method is executed asynchronously.  Calling *this.state* within setState() is unreliable because other setState() methods could finish before the updated clicked setState().  If this was the case, *this.state* wouldn't be reflected properly.

In this situation the functional form of this.setState() should be used.
```jsx
this.setState( (prevState, props) => {
    return {
        clicked: prevState.clicked + 1
    }
});
```
Instead of *this.state*, the *prevState* argument is used.  *prevState* is the true previous state and it's immutable, so any changes made to setState in other methods will not affect it.

Although the bad appraoch works, the best practice of mutating state that relies on the previous state is to use the function form of setState() and the *prevState* argument.

