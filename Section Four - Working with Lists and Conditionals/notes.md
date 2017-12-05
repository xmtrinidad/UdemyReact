# Working with Lists and Conditionals

In this module:
* How to output content conditionally
* How to output lists of data

**Table of Contents**

[Rendering Content Conditionally](#rendering-content-conditionally)    
[Handling Dynamic Content The JavaScript Way](#handling-dynamic-content-the-javascript-way)    
[Outputting Lists](#outputting-lists)    
[Lists and State](#lists-and-state)    
[Updating State Immutably](#updating-state-immutably)    

**Practice Projects**

| Link            | Description |
| --------------- | ----------- |
| [Stack Blitz](https://stackblitz.com/edit/show-hide-conditional) | Practicing Conditional Statements |
| [Stack Blitz](https://stackblitz.com/edit/map-and-add) | Practice with map() to create components |

## Rendering Content Conditionally

1.  Wrap the content in a div that is subject to the condition
    ```jsx
    <div>
        <Person 
          name={this.state.persons[0].name} 
          age={this.state.persons[0].age}>
        </Person>
        <Person
          changed={this.nameChangedHandler}
          click={this.switchNameHandler}
          name={this.state.persons[1].name} 
          age={this.state.persons[1].age}>My Hobbies: Breaking windows
        </Person>
        <Person 
          name={this.state.persons[2].name} 
          age={this.state.persons[2].age}>
        </Person>
    </div>
    ```
2.  Assign a method reference to the button/element that will trigger the conditional logic and create the method above the *render()* function:
    ```js
    togglePersonsHandler = () => {

    }

    render() {
      // ...JSX
    }
    ```
    ```jsx
    <button onClick={this.togglePersonsHandler}>Switch name</button>
    ```

3.  To toggle a property, the condition needs to be set in *state*
    ```js
    state = {
        persons: [
          { name: 'Xavier', age: 28 },
          { name: 'Mooky', age: 30 },
          { name: 'Leslie', age: 28 },
        ],
        otherState: 'some other value',
        showPersons: false
      }
    ```
    *showPersons* is the new property that is initially set to false.  This is the property that will be toggled from the *togglePersonsHandler()* method.


4. Unlike in Angular where directives such as **ngIf* are used, React uses JSX so hiding/showing content uses JavaScript syntax.  Wrapping the ```<div>``` of the conditional block in brackets would be valid javascript code:
    ```jsx
    {<div>
        <Person 
          name={this.state.persons[0].name} 
          age={this.state.persons[0].age}>
        </Person>
        <Person
          changed={this.nameChangedHandler}
          click={this.switchNameHandler}
          name={this.state.persons[1].name} 
          age={this.state.persons[1].age}>My Hobbies: Breaking windows
        </Person>
        <Person 
          name={this.state.persons[2].name} 
          age={this.state.persons[2].age}>
        </Person>
    </div>}
    ```
5.  An *if* statement will not work in this situtation, but using a ternary expression does work
  ```jsx
    {
      this.state.showPersons ? 
        <div>
            <Person 
              name={this.state.persons[0].name} 
              age={this.state.persons[0].age}>
            </Person>
            <Person
              changed={this.nameChangedHandler}
              click={this.switchNameHandler}
              name={this.state.persons[1].name} 
              age={this.state.persons[1].age}>My Hobbies: Breaking windows
            </Person>
            <Person 
              name={this.state.persons[2].name} 
              age={this.state.persons[2].age}>
            </Person>
        </div> : null
    }
  ```
6.  Set logic in toggle method:
  ```jsx
  togglePersonsHandler = () => {
    const doesShow = this.state.showPersons;
    this.setState({showPersons: !doesShow});
  }
  ```

## Handling Dynamic Content The JavaScript Way

The above method of hiding/showing data conditionally isn't optimal.  It can be difficult to keep track of nested conditions and determining which conditions are responsible for what in bigger applications.

There is a cleaner way:

1.  Don't use curly braces like in the previous example and start with the base jsx:
  ```jsx
  <div>
    <Person 
      name={this.state.persons[0].name} 
      age={this.state.persons[0].age}>
    </Person>
    <Person
      changed={this.nameChangedHandler}
      click={this.switchNameHandler}
      name={this.state.persons[1].name} 
      age={this.state.persons[1].age}>My Hobbies: Breaking windows
    </Person>
    <Person 
      name={this.state.persons[2].name} 
      age={this.state.persons[2].age}>
    </Person>
  </div>
  ```

2.  When React renders something to the screen or decides to update the screen it executes the render() method.  Inside the *render()* method is where the conditional statements will be defined.  

  ```jsx
  let persons = null;

  if (this.state.showPersons) {
    persons = (
      <div>
        <Person 
          name={this.state.persons[0].name} 
          age={this.state.persons[0].age}>
        </Person>
        <Person
          changed={this.nameChangedHandler}
          click={this.switchNameHandler}
          name={this.state.persons[1].name} 
          age={this.state.persons[1].age}>My Hobbies: Breaking windows
        </Person>
        <Person 
          name={this.state.persons[2].name} 
          age={this.state.persons[2].age}>
        </Person>
    </div>
    );
  }
  ```
  
  By default *persons* is null.  If *this.state.showPersons* is true, *persons* is set to the jsx code

3.  Inside the return statement, place the *persons* variable inside the jsx:
  ```jsx
  return (
    <div className="App">
      <h1>Hi, I'm a React App</h1>
      <button 
        style={style}
        onClick={this.togglePersonsHandler}>Switch Name</button>
      {persons}
    </div>
  );
  ```

## Outputting Lists

In Angular, a list like below:
```jsx
<Person 
  name={this.state.persons[0].name} 
  age={this.state.persons[0].age}>
</Person>
<Person
  changed={this.nameChangedHandler}
  click={this.switchNameHandler}
  name={this.state.persons[1].name} 
  age={this.state.persons[1].age}>My Hobbies: Breaking windows
</Person>
<Person 
  name={this.state.persons[2].name} 
  age={this.state.persons[2].age}>
</Person>
```

Would be iterated over using an *ngFor directive.  In React, the *map()* method is used to achieve the same result.

The *map()* method iterates over the array within the *state*:
```jsx
state = {
    persons: [
      { name: 'Xavier', age: 28 },
      { name: 'Mooky', age: 30 },
      { name: 'Leslie', age: 28 },
    ],
    otherState: 'some other value',
    showPersons: false
  }
```

To loop over the persons array, define a variable and assign jsx using the *map()* method:

```jsx
persons = (
  <div>
    {this.state.persons.map(person => {
      return <Person 
        name={person.name} 
        age={person.age} />
    })}
</div>
```
The *map()* method iterates over each item in the array and applies the associated function to each item.  In this case, there is a *persons* array and each *person* within that array is iterated over and converted into JSX.

## Lists and State

To manipulate individual items in an array within a map(), access to the index of that item is needed.  The map() method automatically has an index paramter:

```jsx
this.state.persons.map((person, index) => {

});
```

To apply a method to use on this individual item, define it above the render (like any other method) and add the attribute to the component:

```jsx
deletePersonHandler = (personIndex) => {
  const persons = this.state.persons;
  persons.splice(personIndex, 1);
  this.setState({persons: persons});
}
```

```jsx
<div>
    {this.state.persons.map((person, index) => {
      return <Person 
        click={() => this.deletePersonHandler(index)}
        name={person.name} 
        age={person.age} />
    })}
</div>
```
```click={() => this.deletePersonHandler(index)}``` -- Using the arrow function syntax like this makes it easier and more readable to access the index of the clicked component.  Since this method is assigned an *index*, the method it references should expect an index when it is executed.

Inside the *deletePersonHandler()* method, splice() is used to cut out the element at the passed in index, and only that element.  The state is then set to the *persons* constant, updating the state minus the deleted element.

## Updating State Immutably

In JavaScript, object and arrays are reference types.  With state assigned like so:
```jsx
class App extends Component {
  state = {
    persons: [
      { name: 'Xavier', age: 28 },
      { name: 'Mooky', age: 30 },
      { name: 'Leslie', age: 28 },
    ],
    otherState: 'some other value',
    showPersons: false
  }
```

It can be pointed to in other methods:
```jsx
deletePersonHandler = (personIndex) => {
  const persons = this.state.persons;
  persons.splice(personIndex, 1);
  this.setState({persons: persons});
}
```

*const persons* points to the original *state* managed by React.  Using the *splice()* method like above mutates the original data.  Although it doesn't throw an error, it isn't the correct way to manipulate state and can lead to errors in the future for larger scale projects.

The best practice is to create a copy of the data before manipulating it.  There are two ways to do this:

1. Using the *slice()* method
    ```jsx
    const persons = this.state.persons.slice()
    ```
    *slice()* without arguments copies the array and returns a new one.

2. Using the ES6 *spread* operator
    ```jsx
    const persons = [...this.state.persons];
    ```
    The elements in the array are "spread" into a list of elements, which are then added to a new array

After using one of these two approaches to copy the data, any manipulation done to the copy will not affect the original state.

```jsx
deletePersonHandler = (personIndex) => {
  const persons = [...this.state.persons];
  persons.splice(personIndex, 1);
  this.setState({persons: persons});
}
```
Keep in mind that *state* should always be updated in immutably.