# Section 6 - Debugging React Apps

**Table of Contents**

[Understanding Error Messages](#understanding-error-messages)       
[Finding Logical Errors by using Dev Tools and Sourcemaps](#finding-logical-errors-by-using-dev-tools-and-sourcemaps)       
[Working with the React Developer Tools](#working-with-the-react-developer-tools)       
[Using Error Boundaries](#using-error-boundaries)       




## Understanding Error Messages

These are basic error messages logged in the console.  React provides a more detailed error message in the browser window.

## Finding Logical Errors by using Dev Tools and Sourcemaps

Sourcemaps display the code in an app and allow it to be debugged by placing break points on any line of code.  Breakpoints can be stepped into and the associated data can be inspected for errors.

In Firefox, Sourcemaps can be found in the Developer Tools > Debugger tab

In Chrome, Sourcemaps can be found in the Developer Tools > Sources

## Working with the React Developer Tools

Developer tool specifically made for React applications.  Links to Chrome/Firefox extensions can be found on the official [Github Page](https://github.com/facebook/react-devtools)

Once the extension is installed, open the browsers Development Tools and there should be a React tab.  This tab will give a more detailed overview of the React application, including displaying state and props for components.

## Using Error Boundaries

New feature created for React 16 for catching errors from sources out of the developers control. For example, a web server that fails for whatever reason; the developer may not have control over the operation of the web server and cannot guarantee if it is working properly.

By setting an Error Boundary, if a particular component fails, a message can be displayed and only that component is affected.  The rest of the application would continue to function as intended.

Steps for creating an Error Boundary:
1.  Create a new ErrorBoundary folder and ErrorBoundary.js file inside that folder.

2. Create a class component inside the ErrorBoundary.js file (make sure to import React and Component):
    ```jsx
    import React, { Component } from 'react';

    class ErrorBoundary extends Component {

    }
    ```
3. Render an error message based on condition.  To do this, it requires *state* (which is why a class component is needed):
    ```jsx
    class ErrorBoundary extends Component {
        state = {
            hasError: false,
            errorMessage: ''
        }
        // This method is executed when ErrorBoundary throws an error
        componentDidCatch = (error, info) => {
            this.setState({hasError: true, errorMessage: error});
        }
        render() {
            if (this.state.hasError) {
                // Message will be whatever the error is
                return <h1>{this.state.errorMessage}</h1>
            } else {
                // this is what's wrapped inside of the ErrorBoundary
                return this.props.children;
            }
            
        }
    }
    ```

4.  Export the component

5.  Inside the component using the ErrorBoundary, import the ErrorBoundary component

6.  Wrap the component with an <ErrorBoundary> tag:
    ```jsx
    <ErrorBoundary>
        <Person />
    </ErrorBoundary>
    ```
    ErrorBoundary is a *higher order component* that wraps around a component, monitoring it for any thrown errors.

    With <ErrorBoundary> wrapping around the <Person /> component, the key attribute needs to be moved to the ErrorBoundary.

During development mode errors will still present themselves the same way as before, but if the app is in production, the error message will be displayed instead.

Error Boundaries should only be used sparingly; only in situtations where an application could fail.
