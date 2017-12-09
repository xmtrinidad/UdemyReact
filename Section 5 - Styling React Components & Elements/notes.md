# Section 5 - Styling React Components & Elements

In the previous section some React styling techniques were introduced such as inline styles and using an imported CSS file.

However, there is more to styling than these two techniques.  There are also some limitations to them, for example an imported CSS file is not scoped and inline styles can't use media queries.  And neither technique used thus far has been used to adjust styles dynamically.

This section will cover dynamically adjusting styles and working around various restrictions of inline styling.

**Table of Contents**       
[Setting Styles Dynamically](#setting-styles-dynamically)       
[Setting Class Names Dynamically](#setting-class-names-dynamically)       
[Adding and Using Radium](#adding-and-using-radium)       
[Using Radium for Media Queries](#using-radium-for-media-queries)     
[Adding Pseudo Selectors](#adding-pseudo-selectors)       
[Working with Media Queries](#working-with-media-queries)       


## Setting Styles Dynamically

When setting styles dynamically, remember that every is JavaScript.  Any change will involve using JavaScript properties to adjust CSS styles.

Keeping this in mind, take the following example within a *render()* function:
```jsx
const style = {
    backgroundColor: 'green',
    color: 'white',
    font: 'inherit',
    border: '1px solid blue',
    padding: '8px',
    cursor: 'pointer'
};

if (condition) {
    style.backgroundColor = 'red';
}
```
The element where this *style* constant is applied would change based on the condition set.

Everything is JavaScript.


## Setting Class Names Dynamically

Let's say we have the following two classes defined in the App.css file:

```css
.red {
  color: red;
}

.bold {
  font-weight: bold;
}
```

Adding these two classes conditionally to an element will involve the [*join()*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join) method.

Initialize an empty array then set conditions:
```jsx
const classes = [];
if (this.state.persons.length <= 2) {
    classes.push('red');
}
if (this.state.persons.length <= 1) {
    classes.push('bold');
    }
```
Then set the variable/constant as a *className* property on the JSX element whose styles will be affected:
```jsx
<p className={classes.join(' ')}>This is really working</p>
```

If both classes are applied, the *join()* method with the empty space will turn the array from ```['red', 'bold']``` to a joined string ```'red bold'``` and, since this is a className property/attribute, it will be rendered as a *class* by React and placed into the DOM as ```class="red bold"```.


## Adding and Using Radium

From the official [Radium website](http://formidable.com/open-source/radium/):
```
Radium unlocks the power of React & inline styling by enabling support for CSS pseudo selectors, media queries, vendor-prefixing, and much more through a simple interface.
```

Import Radium via NPM:
```
npm install --save radium
```

Import Radium into the file where you want to use it
```js
import Radium from 'radium';
```

Down at the bottom of the file where the component is exported, wrap the component in a *Radium()* function:
```js
// Note that this is called a 'higher order component'
export default Radium(App);
```

Radium can be used on both components created with *class componentName extends Component* and functional components, just be sure to import Radium and wrap the export in a Radium() function.

Once this is done, Radium can be used within inline styles:
```jsx
const style = {
    backgroundColor: 'green',
    color: 'white',
    font: 'inherit',
    border: '1px solid blue',
    padding: '8px',
    cursor: 'pointer',
    ':hover': { // this is Radium
    backgroundColor: 'lightgreen',
    color: 'black'
    }
};
```

You can change a pseudo class selector in JavaScript using Radium using brackets:
```jsx
style[':hover'] = {
    backgroundColor: 'salmon',
    color: 'black'
}
```

In the above example, using the *:hover* pseudo class needs to be wrapped in quotes because otherwise it isn't valid JavaScript syntax.


## Using Radium for Media Queries

## Adding Pseudo Selectors

## Working with Media Queries
