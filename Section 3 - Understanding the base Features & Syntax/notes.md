# Section Three - Understanding the Base Features & Syntax

**Table of Contents**

[Create React App](#create-react-app)    
[Understanding the Folder Structure](#understanding-the-folder-structure)   
[Understanding Component Basics](#understanding-component-basics)   
[Understanding JSX](#understanding-jsx)   
[Creating a Functional Component](#creating-a-functional-component)   
[Outputting Dynamic Content](#outputting-dynamic-content)   
[Working with Props](#working-with-props)   
[Children Property](#children-property)   
[Understanding and Using State](#understanding-and-using-state)   
[Handling Events with Methods](#handling-events-with-methods)   
[Manipulating the State](#manipulating-the-state)   
[Passing Method References Between Components](#passing-method-references-between-components)   
[Adding Two Way Binding](#adding-two-way-binding)   
[Adding Styling with Stylesheets](#adding-styling-with-stylesheets)   
[Working with Inline Styles](#working-with-inline-styles)   

**Practice Projects**

| Link          | Description   |
| ------------- | ------------- |
|[Stack Blitz](https://stackblitz.com/edit/basic-react-components)| Practicing basic components with state |
|[Stack Blitz](https://stackblitz.com/edit/events-and-two-way-binding)| Two way data binding and handling click events |
|[Stack Blitz](https://stackblitz.com/edit/two-way-binding-todo)| More practice with two-way bindings, state and components |


## Create React App
([github](https://github.com/facebookincubator/create-react-app))

The officially recommended tool for creating React Projects.  Install globally with:
```
npm install -g create-react-app
```

Start a new project and live server with the following commands:
```
create-react-app my-app
cd my-app/
npm start
```

## Understanding the Folder Structure

At the root level there are some configuration files:
* package-lock.json lock in the versions of the dependencies being used in the project
* The general package dependencies are found in the package.json file
  * There are three dependencies similar to the script imports from the getting started codepen project:
  ```
  "dependencies": {
    "react": "^16.2.0",
    "react-dom": "^16.2.0",
    "react-scripts": "1.0.17"
  }
  ```
* The node modules holds all the dependencies and subdependencies for the project

### The Public Folder
The public folder is the root folder that gets served to the web server in the end

#### index.html 

The index.html file is a normal html page and the single HTML page for the project.  You won't add more html files to the project.
* This single HTML page is where the script files get injected
* ```<div id="root"></div>``` is where the React application is mounted
* You can add imports and meta tag info to the html file

#### manifest.json

Create React App creates a PWA by default.  The manifest.json file defines metadata about the application

### The Src Folder

The React application

#### index.js

The initial ReactDOM.render() method is found here and accesses the index.html 'root' element.

#### App.js

The first and only React component in the starting project and its associated JSX

## Understanding Component Basics

Typically there is only one ReactDOM.render() method used, the app/root component.

From this component, all other components are nested.

```jsx
class App extends Component {
  render() {
    return (
      <div className="App">
        <h1>Hi, I'm a React App</h1>
      </div>
    );
  }
}
```

This is the app component where everything will be nested

### Defining a component with class

Using a *class* is one way to define a React component
```class App extends Component```

The *extends* keyword is used to inherit from the *Component* class which is imported from the 'react' library along with React itself:
```javascript
import React, { Component } from 'react';
```

The *App* class has only one method, *render()*.  React calls this method to render something to the screen.

Every React component has to return/render HTML code that can be rendered to the DOM and screen.

The App class is exported so other files can use it
```javascript
export default App;
```

## Understanding JSX

If you were to remove the HTML from the render() method and return ```React.createElement()`` and it's associated arguments, it could create the same HTML component.

Since the HTML in JSX isn't really HTML, some keywords would conflict with javascript keywords, which is why, for example 'class' needs to be changed to 'className'

Another restriction to JSX is that it can only have one root element.  For example:

```jsx
<div className="App">
  <h1>Hi, I'm a React App</h1>
</div>
```

Is valid but adding another element at the bototm like so:
```jsx
<div className="App">
  <h1>Hi, I'm a React App</h1>
</div>
<h1>A new Element</h1>
```

Would not be valid.  There are two root elements in this component.

## Creating a Functional Component

1.  Create a new folder that will hold the component files.  The name of the folder should be capitalized
```
Person
```

2.  Inside the component folder, create a javascript file, following the capitalization convention.
```
Person.js
```

3.  Use a javascript function to create a component using JSX and ES6.
```jsx
const person = () => {
    return <p>I'm a Person!</p>
};
```
The javascript function is typically lowercase

4.  Import React
```javascript
import React from 'react';
```

5.  Export the component function
```javascript
export default person;
```

6.  In the App.js file, import the component
```js
import Person from './Person/Person';
```

7.  Add the component as an element into the App component
```jsx
<div className="App">
  <h1>Hi, I'm a React App</h1>
  <Person></Person>
</div>
```

## Outputting Dynamic Content
To out put dynamic content, use curly braces:
```jsx
const person = () => {
    return <p>I'm a Person and I am {Math.floor(Math.random() * 30)} years old!!</p>
};
```

## Working with Props

The component function can take *props* as an argument and properties can be attached to it:
```jsx
const person = (props) => {
    return <p>I'm {props.name} and I am {props.age} years old!!</p>
};
```
For this component, *name* and *age* are properties and can be used with components like so:
```jsx
<Person name="Xavier" age="28"></Person>
<Person name="Mooky" age="30"></Person>
<Person name="Leslie" age="28"></Person>
```

## Children Property

The children property can be used to parse data from component declarations.  For example:
```jsx
<Person name="Mooky" age="30">My Hobbies: Breaking windows</Person>
```

Without accessing the *children* property, 'My Hobbies: Breaking windows' would not be seen.  In order to display it on the screen, refactor the component to use *props.children*:
```jsx
const person = (props) => {
    return (
        <div>
            <p>I'm {props.name} and I am {props.age} years old!!</p>
            <p>{props.children}</p>
        </div>
    )
};
```

## Understanding and Using State

 Defining properties in a non-hardcoded way using the *state* property

 *state* can only be used in components which are created from extending *Component*

```javascript
state = {
    persons: [
      { name: 'Xavier', age: 28 },
      { name: 'Mooky', age: 30 },
      { name: 'Leslie', age: 28 },
    ]
  }
```

State can be accesed from the *render()* method and the component using the properties from state:
```jsx
<Person name={this.state.persons[0].name} age={this.state.persons[0].age}></Person>

<Person name={this.state.persons[1].name} age={this.state.persons[1].name}>My Hobbies: Breaking windows</Person>

<Person name={this.state.persons[2].name} age={this.state.persons[2].age}></Person>

```

## Handling Events with Methods

1.  Add *onClick* to the element.  Note that this on click is is camelcase, unlike regular javascript (onclick).  For JSX, onClick is used as a click handler.

2.  Set *onClick* equal to the method you want to run, but don't include parenthesis.  Adding parenthesis would execute the function immediately.  By passing by reference, this won't happen
```jsx
<button onClick={this.switchNameHandler}>Switch Name</button>
```

3.  Define the method

```javascript
switchNameHandler = () => {
    console.log('was clicked');
  }
```

## Manipulating the State

Dont alter state directly:
```jsx
// DON'T DO THIS: this.state.persons[0].name = 'Javi';
```

Use the setState() method instead
```js
switchNameHandler = () => {
    // DON'T DO THIS: this.state.persons[0].name = 'Javi';
    this.setState({
      persons: [
        { name: 'Javi', age: 28 },
        { name: 'Mooky', age: 30 },
        { name: 'Leslie', age: 27 },
      ]
    })
  }
```

## Passing Method References Between Components

1.  Use on click on component element
```jsx
<p onClick={}>I'm {props.name} and I am {props.age} years old!!</p>
```

2.  Inside the render() method in App.js, add a *click* property
```jsx
<Person
  click={this.switchNameHandler}
  name={this.state.persons[1].name} 
  age={this.state.persons[1].name}>My Hobbies: Breaking windows
</Person>
```

3.  Update *onClick* attribute in component
```jsx
<p onClick={props.click}>I'm {props.name} and I am {props.age} years old!!</p>
```

## Adding Two Way Binding

Uses a special event *onChange* which is fired whenever the value changes

```jsx
<input type="text" onChange={props.changed} value={props.name} />
```

*props.changed* is passed down from the App.js file.  Inside the App.js file is where the logic for the two-way data binding would be defined

```javascript
nameChangedHandler = (event) => {
    this.setState({
      persons: [
        { name: event.target.value, age: 30 },
      ]
    })
  }
  ```

*nameChangedHandler()* is the event that fires when the value changes.  This method has an event parameter and, inside the object that is changing, *event.target.value* captures the input value on every change.

To use the *nameChangedHandler()* method, use it as a component attribute:

```jsx
<Person
  changed={this.nameChangedHandler}
</Person>
```

## Adding Styling with Stylesheets

1.  Add a CSS file to the component folder (Person.css)
2.  The CSS file created is not scoped to the component.  To ensure that styles added within the CSS file do not affect other components, define a component class named after the component and add it to the main div element.
  ```css
    .Person {

    }
  ```
3.  Inside the component javascript file, import the CSS file
```javascript
import './Person.css';
```

## Working with Inline Styles

Sometimes in React apps, style are defind using JavaScript

Define a const variable to hold your styles and place it below *render()* function and assign values then add a *style* property to the element that will use the defined styles
```jsx
render() {
  const style = {
    backgroundColor: 'white',
    font: 'inherit',
    border: '1px solid blue',
    padding: '8px'
  };

  return (
    <button style={style}>Switch Name</button>
  )
}
```
