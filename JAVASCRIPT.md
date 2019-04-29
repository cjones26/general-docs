
# Best Practices / Coding Conventions

*Note: These rules are not applicable in all situations – there are rare occasions we must break convention for convenience.*

All of our JavaScript best practices & coding conventions are based upon the [Airbnb JavaScript style guide]([https://github.com/airbnb/javascript/tree/685f37be39fd01bcfdd349de9acdcd5a50414520](https://github.com/airbnb/javascript/tree/685f37be39fd01bcfdd349de9acdcd5a50414520)) (eslint-config v17.1.0) with some deviations.  These deviations are listed below:

 - [7.1](https://github.com/airbnb/javascript/tree/447466681e9c38858ef81af7a84aada801630352#functions--declarations) – Use named function expressions instead of function declarations

    -   The rationale for this tenet can be found [here](https://stackoverflow.com/questions/15336347/why-use-named-function-expressions).
    -   The style guide recommends we avoid utilizing function declarations. We _do_ in fact utilize function declarations, for example:

```
    // bad
    const short = function longUniqueMoreDescriptiveLexicalFoo() {
    	// …
    };
    
    // good
    const foo = () => { … }
```
  
  

-   [10.1](https://github.com/airbnb/javascript/tree/685f37be39fd01bcfdd349de9acdcd5a50414520#modules--use-them) – Always use modules (import/export) over a non-standard module system

    -   Though I do agree that we must utilize ES6 modules where we can, these are not fully supported in Node at this time, therefore, we will _not_ be utilizing ES6 modules on our backend server module.

```
// bad (except on the Node server backend)
const moment = require('moment');

// good
Import moment from 'moment';
```
-   [10.2](https://github.com/airbnb/javascript/tree/447466681e9c38858ef81af7a84aada801630352#modules--no-wildcard) – Do not use wildcard imports    
    - Some third-party libraries may require the use of a wildcard import.
        
-   [11.2](https://github.com/airbnb/javascript/tree/447466681e9c38858ef81af7a84aada801630352#generators--nope) – Don't use generators for now    
    -   Redux Saga requires the use of generators.
-   [13.4](https://github.com/airbnb/javascript/tree/447466681e9c38858ef81af7a84aada801630352#variables--define-where-used) – Assign variables where you need them, but place them in a reasonable place
    
    -   Prefer grouping all variables at the top of the function or block

```
// bad
function checkName(hasName) {
  if (hasName === 'test') {
    return false;
  }

  const name = getName();

  if (name === 'test') {
    this.setName('');
    return false;
  }

  return name;
}

// good
function checkName(hasName) {
  const name = getName();

  if (hasName === 'test') {
    return false;
  }

  if (name === 'test') {
    this.setName('');
    return false;
  }

  return name;
}
```

-   [19.9](https://github.com/airbnb/javascript/tree/447466681e9c38858ef81af7a84aada801630352#whitespace--no-multiple-blanks) – Do not use multiple blank lines to pad your code
    
    -    Prefer utilizing an extra blank line after variable declarations:
```
// bad
getDate() {
	const today = new Date();
	return today;
}

// good
GetDate() {
	const today = new Date();

	return today;
}
```
  
  

-   [19.13](https://github.com/airbnb/javascript/tree/447466681e9c38858ef81af7a84aada801630352#whitespace--max-len) – Avoid having lines of code that are longer than 100 characters (including whitespace)
    
    -   Prefer upping the limit from 100 to 120 character long lines of code
        
-   [20.2](https://github.com/airbnb/javascript/tree/447466681e9c38858ef81af7a84aada801630352#commas--dangling) – Additional trailing comma
    
    -   TODO: Need to determine how we can ensure that Prettier does not remove these when we format our code
        
-   [23.2](https://github.com/airbnb/javascript/tree/447466681e9c38858ef81af7a84aada801630352#naming--camelCase) – Use camelCase when naming objects, functions, and instances
    
    -   In regards the rationale below, see [23.10](https://github.com/airbnb/javascript/tree/447466681e9c38858ef81af7a84aada801630352#naming--uppercase)
    -   We _do_ want to utilize camelCase when naming objects, functions, and instances, with the exception of application constants (_not_ simply const variables)– these we want to name in all caps, delimited by underscores, for example:
```
// bad
const defaultItemsPerPage = 10;

// good
const DEFAULT_ITEMS_PER_PAGE = 10;
```


Aside from the Airbnb style guide, there are some other, framework-specific guidelines which we must follow when utilizing React, Redux, and Jest:

-   **React best practices**
    
    -   Files should be named with the .js extension
        
    -   Component names should match file names
        
    -   Redux connected container components should have a “Container” suffix, while view components should have a “View” suffix
        
    -   Standard, unconnected components' filenames should match the name of the component itself (no Container/View suffix)
        
    -   Minimize complexity of the render method
        
    -   Use the [prop-types](https://www.npmjs.com/package/prop-types) library to define prop definitions
        
    -   Use stateless, functional components where possible and switch to stateful components only when the logic cannot be handled without stateless
        
    -   Do **not** use hooks at this time – we will look at implementing these in the future once they are more stable within React
        
    -   Do **not** synchronize state and props
        
        -   There are very few occasions where it's necessary to derive state from props – most of the time if we need to derive something from props it can be handled by memoization
            
        -   Please see [here](https://reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html) for more information on how to avoid this anti-pattern
            
-   **Redux best practices**
    
    -   Do **not** store React components (or any sort of function) within Redux state—only serializable objects such as arrays and objects belong in state
        
    -   Use native JS functionality and/or helper functions to make immutable state changes
        
        -   Use the ES6 spread operator _everywhere_ you can
            
    -   Use Redux Saga to perform any asynchronous actions
        
    -   Split action creators, reducers, sagas, and selectors into separate files, and imported into an index.js file—for example:
        
        -   calculatorActions.js
            
        -   calculatorReducer.js
            
        -   calculatorSagas.js
            
        -   calculatorSelectors.js
            
    
-   **React Unit Testing best practices**
    
    -   Use React Testing Library (RTL) to validate what the user observes and experiences
        
    -   All view components (both presentational and container components) should be covered by unit tests
        
    -   Always try to fix a bug with a test
        
    -   **Presentational components**
        
        -   Test whether the component gets rendered and what it renders. Only one test verifying happy path is sufficient.
            
        -   Only unit test props which have some conditional logic surrounding them.
            
        -   Do not write tests for style attributes, unless they have some conditional logic surrounding them.
            
    -   **Container components**
        
        -   Focus on testing functionality as these components render no HTML.
            
        -   Test callback logic if it is more complex than simply calling a bound action creator.
            
        -   Test derived data creation – prefer deriving data in the container component, rather than in the presentational component.
            
    -   **Use the rules [here](https://medium.freecodecamp.org/the-right-way-to-test-react-components-548a4736ab22) for determining what is not worth testing**
        
        -   Do not make your tests brittle by duplicating application code exactly.
            
        -   Will making assertions in the test duplicate any behavior which is already covered by (and the responsibility of) library code? If so, don't test it.
            
        -   From an outsider's perspective ,is this detail important, or is it only an internal concern? Can the effect of this internal detail be described utilizing only the component's public API? If so, don't test it.
            
-   **Redux Unit Testing best practices**
    
    -   Test all reducer logic
        
    -   There usually should not be a need to test all action creators
        
    -   When testing sagas, utiliize Redux Saga Test Plan as opposed to accessing the iterator directly.
