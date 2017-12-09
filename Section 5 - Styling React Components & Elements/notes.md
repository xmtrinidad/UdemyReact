# Section 5 - Styling React Components & Elements

In the previous section some React styling techniques were introduced such as inline styles and using an imported CSS file.

However, there is more to styling than these two techniques.  There are also some limitations to them, for example an imported CSS file is not scoped and inline styles can't use media queries.  And neither technique used thus far has been used to adjust styles dynamically.

This section will cover dynamically adjusting styles and working around various restrictions of inline styling.

**Table of Contents**       
[Setting Styles Dynamically](#setting-styles-dynamically)       
[Setting Class Names Dynamically](#setting-class-names-dynamically)       
[Adding and Using Radium](#adding-and-using-radium)       
[Using Radium for Media Queries](#using-radium-for-media-queries)     
[Enabling and Using CSS Modules](#enabling-and-using-css-modules)     
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

To add inline-style media queries, Raidum has to be used.  There are a few things that need to be added to the overall App component before they will take effect:

First, add the style constant to the component using the media query:
```jsx
const style = {
    // Remember to use quotes to keep the syntax valid
    '@media (min-width: 500px)': {
        width: '450px'
    }
};
```

Next, add the style constant to the component:
```jsx
<div className="Person" style={style}>
    // ...
</div>
```

At this point, an error will occur:
```
Uncaught Error: To use plugins requiring 'addCSS' (e.g. keyframes, media queries), please wrap your application in the StyleRoot component.
```

*StyleRoot* is a Radium component and, when applying media queries using Radium, needs to be wrapped around the overall App component.

Before the StyleRoot component can be used, it needs to be imported:
```jsx
import Radium, { StyleRoot } from 'radium';
```

Now the entire App component can be wrapped with the StyleRoot component:
```jsx
<StyleRoot>
    <div className="App">
        
        // ...other components

    </div>
</StyleRoot>
```

With StyleRoot added, the media query inside the component should work.

## Enabling and Using CSS Modules

*This section doesn't use Radium.  Remove it and any components and imports associated with it if they are implemented within the App.*

Another way of scoping CSS files that doesn't require Radium (which is for inline-styles) would be through the use of CSS Modules.

Using CSS Modules would allow styles to be imported and assigned to any JSX elements and wouldn't override other styles even if they share the same class names with other elements in the App.

The following steps outline how to enable CSS modules:

1.  The build configuration of the project needs to be changed.  Using the ```npm run eject`` command to allow access to the underlying configuration files that React uses.

    *Enabling access to the configuration files doesn't break the application, but editing files should be handled with care as any changes can introduce errors.*

2.  Two new folders are now available, *scripts* and *config*.  The webpack config inside the config folder is what will be used to enable CSS modules.

3.  Inside the *webpack.config.dev.js* file, scroll down to module then search for the .css extension.  It will look like ```test: /\.css%/``` and have several other properties associated with it.

4.  Within this extension, there is an *options* property that will be edited
```js
test: /\.css$/,
    use: [
        require.resolve('style-loader'),
        {
            loader: require.resolve('css-loader'),
            options: {
                importLoaders: 1,
                modules: true, // Add this
                localIdentName: '[name]__[local]__[hash:base64:5]' // Add this
            },
        }
```

```modules: true``` and ```localIdentName: '[name]__[local]__[hash:base64:5]'``` need to be added

5.  Inside the *webpack.config.prod.js* folder the same config from above needs to be applied, but don't override *minimize* or *sourceMap*:
```js
test: /\.css$/,
    use: [
        require.resolve('style-loader'),
        {
        loader: require.resolve('css-loader'),
        options: {
            importLoaders: 1,
            modules: true,
            localIdentName: '[name]__[local]__[hash:base64:5]'
        },
    }
```

6.  With this config now in place, classes from a CSS file can be imported into a component
```js
import classes from  './App.css';
```
*classes* can be any name.  It refers to all the classes in the component CSS file.

7.  The CSS class from the imported CSS file can be used using Object syntax: 
```js
<div className={classes.App}>

</div>
```
*App* is the name of the CSS class and any styles defind within the CSS file with the class of *.App* will be applied to the element.

8.  Restart the application to apply configuration

Using CSS modules will make CSS component files truly unique and prevent errors from other components containing the same class name.

## Adding Pseudo Selectors

Using CSS Modules, the component CSS files can be used along with normal CSS syntax.  Class styles can be applied the same way that inline styles were used with Radium:

```jsx
<button 
    className={btnClass}>Toggle Persons
</button>
```

*btnClass* would be defined using the import ```btnClass = classes.Red;```

## Working with Media Queries

As before, using CSS modules allows the use of regular CSS files and syntax.  Simply adding a media query to the class being affected, then using the class in the component will make the media query work.