# Section Project - Verb List

Project Link: [Verb List](https://stackblitz.com/edit/section-four-practice)

**Table of Contents**

[Goals](#goals)       
[Creating a List Component](#creating-a-list-component)       
[Key Warning](#key-warning)       
[Two-way Binding](#two-way-binding)       
[Conditional Validation](#conditional-validation)       
[Removing List Items](#removing-list-items)       

## Goals

* Learn how to create a list component, iterating over items in state (Verb List)

* Practice two-way binding (Typing in input, show verb translation under the Spanish verb)

* Set conditional validation based on input (If the English translation is correct display 'Correct', else show nothing)

* Flexible lists (Display multiple cards with input boxes and manipulate only the card that is being changed)

* Hide/show Verv List items conditionally (When the correct answer is typed into the Card component input box, remove item from list)

## Creating a List Component

What I wanted to do was create a list of verbs like so:
```html
<ul>
    <li>Verb 1</li>
    <li>Verb 2</li>
    <li>Verb 3</li>
    <li>Verb 4</li>
</ul>
```

I've never done this in React and, up to this point, hasn't been covered in the tutorial series.  I found the solution from the official [React Docs](https://reactjs.org/docs/lists-and-keys.html) that outlines out to make lists.

For this project, the list is generated from a component called VerbList.  The state is defined like so:
```jsx
this.state = {
    verbs: [
        {id: 'af3', verb: 'hablar', translation: 'to speak'},
        {id: 'fdf', verb: 'ver', translation: 'to see'},
        {id: '3dsf', verb: 'tener', translation: 'to have'},
        {id: 'ayhj', verb: 'querer', translation: 'to want'}
    ]
};
```

The VerbList component is placed inside the App component and has a *verbs* property that gets passed into it an array of verbs from the state:
```jsx
<div>
    <h1>Verb List</h1>
    <VerbList verbs={this.state.verbs}/>
</div>
```
Inside the VerbList component, the verbs are accessed from *props* and mapped over to create list item elements with a unique key taken from the verb id and the verb itself being used as the list item.  This array of list items is then added to the component jsx and returned:
```jsx
const verbList = (props) => {
  const verbs = props.verbs;
  const listItems = verbs.map((v) =>
    <li key={v.id}>{v.verb}</li>
  );

  return (
    <ul className="verb-list">
      {listItems}
    </ul>
  )
}
```
The result is an unordered list that creates list item elements from all the verbs listed in the App state.



## Key Warning

While creating the components in this project I was running into a key not being defined warning.  For my Card component, I was trying to assign a *props.key* to the label element and the input id the same key.  This was so the input and label would match and be more accessible.

However, the *key* wasn't eliminating the error and, after doing some research, I found a solution where using an *id* is better.  Not only because *id* is defined in state, but also because a *key* isn't really a prop.  Giving each list item a key is used by React to know which elements to update in the DOM.

The solution can be found [here on Stack Overflow](https://stackoverflow.com/questions/33661511/reactjs-key-undefined-when-accessed-as-a-prop)

## Two-way Binding

The two way binding functionality I wanted to implement was to update the English translation of the Card component underneath the Spanish verb.

Initially the English translation is hidden because the input is empty.

To implement this functionality, first the component Card needs two props, *id* and *changed*

```jsx
<input 
    id={props.id} 
    onChange={props.changed} />
```
The id is needed so that that individual card is updated and the others are unaffected.  The *onChange* attribute is used to detect changes to the input.  Inside the App component is where these props are defined.

Each Card component has a *changed* attribute and *userInput* attribute.  The App state was refactored to hold a userInput key and value set to an empty string (this is the value that will get updated).
```jsx
this.state = {
    verbs: [
        {id: 'af3', verb: 'hablar', translation: 'to speak', userInput: ''},
        {id: 'fdf', verb: 'ver', translation: 'to see', userInput: ''},
        {id: '3dsf', verb: 'tener', translation: 'to have', userInput: ''},
        {id: 'ayhj', verb: 'querer', translation: 'to want', userInput: ''},
    ]
};
```
```jsx
const verbs = this.state.verbs;
    const listItems = verbs.map(v => {
      return <Card
        changed={(event) => this.userInput(event, v.id)}
        userInput={v.userInput} 
        verb={v.verb}
        key={v.id}
        id={v.id} />
});
```

The *changed* attribute gets the event fired from the updated input value within the Card component and passes it into a method named *userInput()*.  This method takes in an event and the id of the verb being updated.

The *userInput()* method looks similar to the example from the tutorial:
```jsx
userInput = (event, id) => {
    // Get the index of the verb being affected by id
    const verbIndex = this.state.verbs.findIndex(v => {
      return v.id === id;
    });

    // Make a copy of the verb being affected as to not affect the original state
    const verb = {...this.state.verbs[verbIndex]};

    // Update the value of the verb based on the event target value
    verb.userInput = event.target.value;

    // Make a copy of the entire verbs array as to not affect the original state
    const verbs = [...this.state.verbs];

    // Update the verb at the specified verbIndex with the updated verb object
    verbs[verbIndex] = verb;

    // Set state with the updated copy of verbs
    this.setState( {verbs: verbs} );
}
```

After all of this is implemented, each value should update dynamically and only the specific Card component should be affected.

## Conditional Validation

To get the validation I was going for, I gave the Card component a *status* attribute and assigned it a *checkStatus()* method that takes in the affected verb as a parameter:
```jsx
<Card
    status={this.checkStatus(v)}
    changed={(event) => this.userInput(event, v.id)}
    userInput={v.userInput} 
    verb={v.verb}
    key={v.id}
    id={v.id} />
```
```jsx
checkStatus = (verb) => {
    if (verb.translation === verb.userInput) {
        return true
    }
}
```
The checkStatus() method checks if the verb translation matches the userInput and, if it does, it returns true.

Inside the Card component, the condition is set by accessing *props.status*
```jsx
const showCorrect = null;
  if (props.status) {
    showCorrect = (
      <div>
        <p>Correct!</p>
      </div>
    )
}
```

If *props.status* is true, the *showCorrect* variable is set to the associated jsx.  The showCorrect variable is placed inside the component's jsx:
```jsx
return (
    <div>
      <h2>{props.verb}</h2>
      <p>{props.userInput}</p>
      <div>
        <label htmlFor={props.id}>Enter Translation</label>
        <br />
        <input 
          id={props.id} 
          onChange={props.changed} />
      </div>
      {showCorrect}
    </div>
  )
```

## Removing List Items

To hide list items conditionally -- in this case, hiding items that are correct -- some refactoring to the VerbList listItems variable was needed:
```jsx
const listItems = verbs.map((v) => {
    if (v.translation !== v.userInput) {
        return <li key={v.id}>{v.verb}</li>
      }
}
```
As the verbs are mapped over, if the translation matches the userInput, it isn't added to the jsx return statement



