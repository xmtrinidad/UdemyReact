# Section One - Getting Started: What is React

"A Javascript Library for building User Interfaces"

* React apps run in the browser, not on the server.  By not having to wait for a server response, things happen much faster.
* User Interfaces describes components.  Like Angular, React utilizes components as building blocks to make apps and web pages.

## React CDN

The latest version of the React library can be found at the [installation page](https://reactjs.org/docs/installation.html)

There are two external scripts used:
```html
<script crossorigin src="https://unpkg.com/react@16/umd/react.production.min.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.production.min.js"></script>
```

* React itself is the logic for creating components
* React DOM is about rendering these components to the real DOM

## Babel & React

React uses many next generation Javascript features.  In order for these features to be used, they need to be transpiled down to something the browser understands using Babel.

## JSX

A special syntax React uses to allow HTML inside of Javascript.  Below is an example of a JSX function
```js
function Person() {
  return (
    <div className="person">
      <h1>Xavier</h1>
      <p>Your Age: 30</p>
    </div>
  );
}
```
Babel converts this into useable Javascript behind the scenes.

To turn this function into a React component, it needs to be rendered to the screen

```html
<div id="p1"></div>
```

```js
ReactDOM.render(<Person />, document.querySelector('#p1'));
```

The html div with an id of "p1" is where the created React component will go.  To turn the JSX function into a component, ReactDOM.render() is used.

The name of the JSX function is passed in as an HTML element:
```html
<Person />
```
Then, as the second argument, the querySelector is used to get the element to be replaced from the DOM.
```js
document.querySelector('#p1')
```

In other words, the ReactDOM.render() method is saying:

"I want to render a Person component using the Person function and replace the selected DOM component with this new Person component."

Since "class" is a keyword in Javascript, if you want to give HTML elements inside of Javascript a class, you have to use *className* to apply any classes to the element.

Also keep in mind that by creating a new html element ```<Person />``` it may lose any styling from the class because the element becomes a block element.

## Dynamic Components

The Person function can take an argument and that argument can be used on the ReactDOM.render() method to create dynamic data

```js
function Person(props) {
  return (
    <div className="person">
      <h1>{props.name}</h1>
      <p>Your Age: {props.age}</p>
    </div>
  );
}

ReactDOM.render(<Person name="Xavier" age="30" />, document.querySelector('#p1'));
```
Passing an argument makes it easier to create a new component by simply calling the ReactDOM.render() method again and passing in new arguments and a HTML DOM element to replace:
```js
ReactDOM.render(<Person name="Caleb" age="29" />, document.querySelector('#p2'));
```

## Best Practice for Rendering Components

Previously, the ReactDOM.render() component is used twice and would be used as many times as necessary.  To cut down on the amount of times ReactDOM.render() is used, the HTML file can contain one app element:

```html
<div id="app"></div>
```

Inside the Javascript file, a *const* can be defined using JSX and the associated HTML:
```js
const app = (
  <div>
    <Person name="Xavier" age="28" />
    <Person name="Caleb" age="29" />
  </div>
);
```
Then the app variable can be mounted to the ReactDOM.render() method:
```js
ReactDOM.render(app, document.querySelector('#app'));
```

## Examples
The example used for this doc can be found at my [codepen page](https://codepen.io/xmtrinidad/pen/OOoQoX)

Another example can be found [here](https://codepen.io/xmtrinidad/pen/YEJrPO)

* When using brackets for element attributes (like the src and link attributes), remove the quotations for the content to appear:
```javascript
src={props.img}
```

