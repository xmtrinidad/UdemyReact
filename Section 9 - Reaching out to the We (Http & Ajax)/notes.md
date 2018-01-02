# Section 9 - Reaching out to the Web (Http & Ajax)

**Table of Contents**       
[Understanding Http Request in React](#understanding-http-request-in-react)    


## Understanding Http Request in React

Typically, when the React app and Server are communicating and if it's a single-page application, they don't communicate by exchanging HTML pages.  Instead, JSON data is returned.

The server is typically a RESTful API that gets/sends data.

## Introducing Axios

The practice project for this section uses the 3rd party package [Axios](https://github.com/axios/axios) for making HTTP requests.

## Creating a Http Request to GET Data

Getting data from an API involves the use *componentDidMount()* lifecycle hook.  This is where API requests should be made:
```jsx
componentDidMount () {
    axios.get('https://jsonplaceholder.typicode.com/posts')
        .then(response => {
            this.setState({posts: response.data});
        });
}
```

The above code uses axios to make a get request to the URL provided as a parameter (remember to import ```import axios from 'axios';```).  The get() method is Promise based so using *.then()* and the *response*, which is the get request data, the JSON data can be accessed and assigned to value.