# Working with Lists and Conditionals

In this module:
* How to output content conditionally
* How to output lists of data

**Table of Contents**

[Rendering Content Conditionally](#rendering-content-conditionally)    
[Handling Dynamic Content The JavaScript Way](#handling-dynamic-content-the-javascript-way)    

**Practice Projects**

| Link            | Description |
| --------------- | ----------- |
| [Stack Blitz](https://stackblitz.com/edit/show-hide-conditional) | Practicing Conditional Statements |

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
    ```js
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
  ```js
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
  ```js
  togglePersonsHandler = () => {
    const doesShow = this.state.showPersons;
    this.setState({showPersons: !doesShow});
  }
  ```

## Handling Dynamic Content The JavaScript Way

The above method of hiding/showing data conditionally isn't optimal.  It can be difficult to keep track of nested conditions and determining which conditions are responsible for what in bigger applications.

There is a cleaner way:

1.  Don't use curly braces like in the previous example and start with the base jsx:
  ```js
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

  ```js
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
  ```js
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