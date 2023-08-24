---
id: mwe7jze1wvzisp6nka6koym
title: FE Module 11 - React Basics
desc: "Winc FE module 11"
updated: 1692873474647
created: 1692449957678
---

[documentation](https://react.dev/)

### Introduction

#### Benefits:

- less code more functionality compared to JS only approach
- rel. easy: combines JS and HTML concepts
- re-usable: uses components as main building blocks for applications
- performance: uses virtual DOM, compares changes in the app and updates those changes in the real DOM (instead of updating all components)
- possibility of creating both web and mobile (React native framework) apps
- good debugging tools (React Dev Tools in Chrome)

NOTE: it ships a lot of JS so loading time could be slow  
SOLUTION: code splitting > only code that is needed will load server side rendering > sends a fully rendered page as a static HTML markup

#### Features:

React adds a layer of abstraction over using JS directly on the DOM

- Components:  
  UI components are the building blocks of a bigger UI >> easy debug, re-use of components in multi page apps

- Props:  
  using properties we can pass functions and data to other components >> dinamic/interacting components

- State:  
  keep track of values by storing them in a "state" object

- JSX:  
  components use JSX (JS XML) which is an extension to JS - combines parts of HTML and JS

- Virtual DOM:  
  higher version of DOM - it can be uploaded independently of the real DOM  
  it is not able to make changes on the screen (it has all the props as the real DOM however)

if change in React **&rArr;** new version of V-DOM **&rArr;** periodically compared to the real DOM and updates whenever  
there is a diff === RECONCILIATION

#### Other frameworks:

- Angular (old but receiving major improvements lately
- Vue
- Backbone
- Ember
  etc.

#### Drawbacks in React:

- documentation is less beginner friendly and not very structured
- requires a lot of planning (because of its flexibility)
- state management is not perfect

[Angular](https://www.ava.codes/posts/react-alternatives#toc-angular-a-stable-framework-based-on-typescript)  
[Vue](https://www.ava.codes/posts/react-alternatives#toc-vue-js-a-framework-for-fast-ui-building)

### 01. Setup

#### Steps

1. `pnpm create vite@latest my-first-app --template react`
2. `cd my-first-app` >> `pnpm install` => installs dependencies
3. edit Vite config
4. `pnpm run dev` => runs the app via **Vite**-server
5. `pnpm eslint --init` => set up Eslint
6. edit `eslintrc` >>

```javascript
"rules": {
  "react/prop-types":0
}
```

7. `src/App.js` is the file that's running a code at --localhost

#### Files and Directories, Organize a project:

can be done based on <u>types</u>, features, pages, etc.

- create a `components` folder inside `src` (for reusable comps)
  /some comps have child-parent realationship - these can be grouped in a folder inside `components`
- all generic UI comps will be grouped in a `ui` folder
- `pages` folder for page files, can add sub-folders for pages storing the page components, 1xuse comps

<img src="./winc-fe-notes-figures/react-project-str.jpg" title="React project file structure" alt="Basic React project structure" width="50%" height="50%"/>

#### Importing and Exporting files

[MDN documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)

Ways to use import in React:

1. import a default export

```javascript
export default App;
//////////////////////////////
import App from "./App";
```

2. when export is not a default exp. << **RECOMMENDED**

```javascript
export const App = () => {...};
//////////////////////////////
import {App} from "./App"
```

To import multiple modules:  
`import React, {useState} from 'react';`

### 02. JSX (JavaScript XML)

It is a combination of JS and HTML  
Allows for an easier DOM manipulation, but with an extra layer of abstraction,  
it is transformed into JS and HTML which the browser can understand  
Example code of a simple component:

```javascript
import './App.css';

function App() {
  const greeting = "Hi, this is an app made with React"; // variables defined outside the `return()`
  const description = <p>I think it holds an interestig future</p>;

  return (
    <div className="App">
      <h1> Welcome! </h1>
      <p>{greeting}</p> // variable is a string which will be the content within the `<p>` tags
      {description} // tags saved in the variable, injecting the variable here without any tag
    </div>
  );
}

export default App;
```

JSX code goes inside `return ()`

### 03. Rendering Components

#### Rendering

React uses a render function that takes two arguments: HTML code and an HTML element.

It is to display HTML code inside the specified HTML element. I.e.: (from my-first-app/src/main.jsx)

```javascript
ReactDOM.createRoot(document.getElementById("root")).render(
  // define root DOM node
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

- everything inside root is handled by React (almost always a single root DOM node / React app)
- `render()` **&rArr;** to render a React element:
  - pass the root DOM element (`document.getElementById('root')` to `ReactDOM.createRoot()`
  - then use the **render function** on this root element
- the React DOM uses a Virtual DOM
  - the V-DOM is a lighter, in memory version of the DOM
  - can be updated without effecting the DOM
  - contains the sam properties, but it can't make changes on the screen
  - React detects the elements that were changed and updates the DOM (uncanged DOM elements will not render again)
    > change in React **&rArr;** V-DOM render **&rArr;** React compares V-DOM and DOM periodically **&rArr;** when diff. DOM is updated (reconciliation process)

#### Components

A React app consists of many different components.  
These can be reused if needed.

Components can be defined as <u>**functions**</u> (easier to use and maintain) or **classes**.

Example for a **React Function Component**:

```javascript
import './App.css';  // import styles

export const App = () => {  // export App component
  const hello = "Hello Wincer!";

  return (  // return JSX
    <main>
      <h1>{hello}</h1> // inject JS code by wrapping the variable name in {}, the value will render as an <h1>
    </main>
  );
};
```

It is possible to render a component inside another component

```javascript
import './App.css';

const Button = () => {  // define a Button component within the same file (can also be imported, :warning: name is capitalized by conv.
  return (
    <button className="Button">Press me!</button>  // note: className (because 'class' is a reserved word in JS)
  );
};

export const App = () => {
  const hello = "Hello Wincer!";

  return (
    <main>
      <h1>{hello}</h1>
      <Button />  // to use inside "App", wrap "Button" in < /> when adding it to the JSX code, note: capitalized name to distinguish from <button>
    </main>
  );
};
```

#### Fragment

:warning: **in React only one element can be returned from a component**  
SOLUTION: wrap components in a container such as `<div>` or in a **React Fragment**  
Fragment syntax: `<React.Fragment>...</React.Fragment>` or `<>...</>`  
Example:

```javascript
export const App = () => {
  return (
    <>
      <Header />
      <Footer />
    </>
  );
};
```

### 04. Props

React properties are similar to arguments and parameters in JS.
We can pass props to components, and use them in those components, similarly how we would do with param/args in JS functions

Passing props to components:  
when rendering a component, pass props with HTML attr. like syntax.  
Example code for passing props to components:

```javascript
export const App = () => {
  const greeting = "Welcome to our cafe!";
  const coffee = "Coffee";

  return (
    <div className="App">
      <h1>{greeting}</h1>
      <DrinkButtons drinkOne={"Tea"} drinkTwo={coffee} />
    </div>
  );
};
```

Using props inside components:  
props are esentially JS objects, we can access them by creating the parameter **props** and referring/accessing them as object.Key.  
Example code for using prop inside components:

```javascript
export const DrinkButtons = (props) => {
  return (
    <>
      <h2>Would you like tea or coffee?</h2>
      <div className="button-group">
        <button className="button">{props.drinkOne}</button>
        <button className="button">{props.drinkTwo}</button>
      </div>
    </>
  );
};
```

### 05. Conditionals

Inside JSX we cannot use statements, we can only use expressions.  
This results in limited syntax for conditial logic.  
Conditional logic is very important for dinamic applications  
(i.e. do not render serach result without a search term, empty components will crash the app)

Solutions:

1. use conditional statement to render a component when there is data available
2. use conditional statement inside the search result component to only try to use data when available

#### Inside JSX

Syntax: we embedd JS expressions in a `{}` block.

##### Using the **`&&` operator** instead of **`if` statement**

The left side of `&&` must be truthy for the right side expression to render.  
(true && expression will evaluate to expression in JS **&rArr;** falsy lhs will not render rhs)

```javascript
export const App = () => {
  const greeting = "Welcome to our cafe!";
  const userDrink = undefined; //undefined as value is used only for representation

  return (
    <div className="App">
      <h1>{greeting}</h1>
      <DrinkSearch availableDrinks={availableDrinks} />
      {userDrink && <DrinkChoice drink={userDrink} />} // if lhs is truthy the
      component inside `()` will render
    </div>
  );
};
```

##### In JSX we can use the ternary operator to replace the if else statement.

```javascript
export const App = () => {
  const greeting = "Welcome to our cafe!";
  const userDrink = undefined; // undefined as value is used only for representation

  return (
    <div className="App">
      <h1>{greeting}</h1>
      <DrinkSearch availableDrinks={availableDrinks} />
      {userDrink ? <DrinkChoice drink={userDrink} /> : "Please select a drink"}
    </div>
  );
};
```

##### We can also use the ternary operator to dynamically switch between CSS classnames.

```javascript
export const App = () => {
  const greeting = "Welcome to our cafe!";
  const userDrink = undefined; // undefined as value is used only for representation

  return (
    <div className="App">
      <h1 className={userDrink ? "small-header" : "big-header"}>{greeting}</h1>{" "}
      // makes the header small if user have selected a drink
      <DrinkSearch availableDrinks={availableDrinks} />
      {userDrink ? <DrinkChoice drink={userDrink} /> : "Please select a drink"}
    </div>
  );
};
```

#### Conditional return of JSX

##### We can use an `if else` statement outside of the JSX code i.e. in a function

```javascript
export const App = () => {
  const greeting = "Welcome to our cafe!";
  const userDrink = undefined; // undefined as value is used only for representation

  const userChoice = (drink) => {
    // this function returns either a component or a string
    if (drink) {
      return <DrinkChoice drink={drink} />;
    } else {
      return "Please select a drink";
    }
  };

  return (
    <div className="App">
      <h1>{greeting}</h1>
      <DrinkSearch availableDrinks={availableDrinks} />
      {userChoice(userDrink)} // we can use the function inside the JSX
    </div>
  );
};
```

##### We can create a component out of the above function

```javascript
// UserChoice component
const UserChoice = (drink) => {
  if (drink) {
    return <DrinkChoice drink={drink} />;
  } else {
    return "Please select a drink";
  }
};

export const App = () => {
  const greeting = "Welcome to our cafe!";
  const userDrink = undefined; // undefined as value is used only for representation

  return (
    <div className="App">
      <h1>{greeting}</h1>
      <DrinkSearch availableDrinks={availableDrinks} />
      <UserChoice drink={userDrink} /> //
    </div>
  );
};
```

### 06. State

It is a collection of data that stores the state of a page, form or application  
/Data is usually saved in the browser's cash **&rArr;** this improves browsing experience (faster loading)/

- allows (with JS together) for an interactive and dynamic website
- used to track and change states across multiple components (on single page apps)
- state is linked to a single component, but can be updated by user actions inside other child components
- state within a component helps to keep track of dynamic data (that can be changed by user action, timers, events: updates)
- in a state we can keep track of any type of variable (string, boolean, etc.)
- state comes with a function to change the state - in functional components this is done by **React hooks**
- after every change in state React will re-render the component in question

To be able to work with states, we need to import the React hook: **`useState`** (from the React library)

```javascript
import { useState } from "react";

export const DrinkButtons = () => {
  const [selectedDrink, serSelectedDrink] = useState("Tea"); // selectedDrink is a state var with an init val of 'Tea'
  // setSelectedDrink is a function to change the value of the state: selectedDrink
  return <p>Your choice: {selectDrink}</p>; // here we use the state variable to be displayed
};
```

#### Stateful and Statless Components

once we us state in a component it becomes a 'stateful' component / otherwise it is stateless  
**A common pattern in React:**

- a stateful component that maintains it's own state
- state is passed down to child components
- different components have diff purpose: some are handling logic, some are keeping track of state, others displaying contennt, etc.
  > it is useful to keep theese things separated (i.e.: UI comps **&rArr;** displaying content **&rArr;** no state tracking -- it would impede reusability)

```javascript
import { useState } from "react";

// Stateful parent component
export const FruitSelection = () => {
  const [fruit, setFruits] = useState("Apple");

  return <>{fruit && <ShoppingCart content={fruit} />}</>;
};

// Stateless child component
export const ShoppingCart = ({ fruit }) => {
  return <p>Shopping cart: {fruit}</p>;
};
```

#### Using Multiple States

- it is best practice to store simple values in the state of a component (do not put all the local state data in a single object)
- for multiple values, use the useState hook multiple times for multiple variables

```javascript
import { useState } from "react";

export const GameScore = () => {
  const [homeTeamScore, setHomeTeamScore] = useState(0);
  const [visitorTeamScore, setVisitorTeamScore] = useState(0);

  return (
    <>
      <p>Home: {homeTeamScore}</p>
      <p>Visitor: {visitorTeamScore}</p>
    </>
  );
};
```

#### Using Conditionals to Handle Empty States

it is common practice to pass state as prop from one component to another

> :warning: state might not be the object that the component expects **&rArr;** TypeError. I.e.: you pass an empty state and data is expected. This could also happen when a component gets rendered before receiving the data

A **conditional statement** can be a solution to this (se code snippets of stateful/stateless components as an exemple to this as well)
Another way to handle potential empty states is using **optional chaining** . (used with object wich might or might not have a certain key that weare trying to access. **&rArr;** instead of throwing an error it will return an 'undefined' and the app will not stop working.  
Example for conditional chaining:

```javascript
export const DrinkChoice = ({ drink }) => {
  return (
    <>
      <p>{drink?.name}</p> // if drink name doesn't exixt &rArr; returns
      undefined
    </>
  );
};
```

#### The Brackets

When we declare a state variable it returns an array with two items

- the first item of this array is the current state of the component
- the second item is a function to change the value of the state

the `[ ]` in the example code is for array destructuring (more practical to use destr.):

```javascript
const [drink, setDrink] = useState("Tea");

// code above is technically the same as the code below
const drinkStateVariable = useState("Tea"); // returns an array
const drink = drinkStateVariable[0]; // firs array item
const setDrink = drinkStateVariable[1]; // second array item
```

> :bulb: the useState hook stores the components state outside the component. When the component re-renders it will not loose its state this way &rArr; if we just declared a variable `setDrink` and re-assigned a new state value to that, the new value would be lost when the component re-renders, because:
>
> - the component is a JS function that returns JSX which is called on re-render
> - the re-render will reset the variable's value to the initial one
> - any value that was given after the initial declaration will be lost

#### Lifting State Up

used when a state is implemented in a component, but we need it in another or multiple sibling component(s)  
 &dArr;  
we can "lift the state up" to the sibling component's closest common ancestor  
 &dArr;  
then, we can pass the state variable and/or function as props to the child components  
Example code:

```javascript
// implementing the state inside <DrinkButtons/> first
import { useState } from 'react';

export const DrinkButtons = () => {
  const [selectedDrink, setSelectedDrink] = useState('Tea');

  return (<p>Your choice: {setSelectedDrink}</p>);

// lifting the state up (<App/>, parent component)

export const App = () => {
  const [userDrink, setUserDrink] = useState('Coffee'); // state is moved from <DrinkButtons/> to <App/>
  const greeting = 'Welcome to our cafe!';

  return (
    <div className="app">
      <h1>{greeting}</h1>
      <DrinkButtons selectDrink={setUserDrink} />  // passing setUserDrink as prop to <DrinkButtons/>
      {userDrink && <DrinkChoice drink={userDrink} />}
    </div>
  );
};
```

The `<App/>` is now stateful and passes its state to their stateless children components: `<DrinkButtons/>` and `<DrinkChoice/>`.

#### Colocating State

It is technically the opposit of "lifting state up". It happens that the state was implemented in a component higher in the hierarchy, than the only component where the state is actually used &rArr; it makes sence to move the state in the component where it is used.

- helps with readability
- improves performance
  I.e.:

```javascript
import {useState} from 'react';

export const ClassRoom =  ({classTitle}) => {
  const [topic, setTopic] = useState('React: state colocation'); // state implemented in 'ClassRoom' but used in 'Topic'

  return (
    <>
      <p>Welcome to the class, {classTitle}</p>
      {topic && <Topic topic={topic} setTopic={setTopic} />} // we pass topic and setTopic to <Topic/> as props
    </>
  );
};

export const Topic = ({topic, setTopic}) => { // we pass topic and setTopic to <Topic/> as props
  return (
    <>
      <label>Change topic:</label>
      <input type="text" onChange={() => setTopic(event.target.value)} /> // this will be explained in a later topic
      <p>Today's topic: {topic}</p>
    </>
  );
};

// in the code above we only use 'topic' state var. and 'setTopic' in the  <Topic/> component >> move it to <Topic/> if this is the only place where we will use it

/////////////////////////////////////////////////////////////////////////////
import {useState} from 'react';

export const ClassRoom =  ({classTitle}) => {

  return (
    <>
      <p>Welcome to the class, {classTitle}</p>
      <Topic />
    </>
  );
};

export const Topic = () => {
  const [topic, setTopic] = useState('React: state colocation'); // state only used with 'Topic' better to implement it here

  return (
    <>
      <label>Change topic:</label>
      <input type="text" onChange={() => setTopic(event.target.value)} />
      {topic && <h2>Today's topic: {topic}<h2/>}
    </>
  );
};
```

### 07. React Developer Tools in Chrome (install plugin in chrome &rArr; inspect element(s) &rArr; Components or Profiler)

- plugin icon is blue in case of a production react
- the icon is red in case of an app in developement
- inspecting an element &rArr; we can look at components and prop values etc. and also change them in the browser to see the effect

### 08. Lists, Loops and Keys

Usecase example: to render multiple search results as a list (multiple items of the same component type)
We can use **array methods** for this purpose.

#### Array.map()

to loop over an array and render a component for each item. I.e.:

```javascript
// create a groceries list from the array
const groceries = ["Apple", "Orange", "Pear"];

// option-1: use .map() to create a list of JSX nodes >> rendered by returning from the component.
export const GroceryList = () => {
  const groceryItems = groceries.map((item) => <li>{item}</li>);
  return <ul>{groceryItems}</ul>;
};

// option-2: render a component and pass the array items as props
export const GroceryList = () => {
  const groceryItems = groceries.map((item) => <GroceryItem item={item} />);
  return <ul>{groceryItems}</ul>;
};

// option-3: we can return the list items directly without assigning it to a variable
export const GroceryList = () => {
  return (
    <ul>
      {groceries.map((item) => (
        <li>{item}</li>
      ))}
    </ul>
  );
};

// Output:
// <ul>
//   <li>Apple</li>
//   <li>Pear</li>
//   <li>Orange</li>
// </ul>
```

#### Keys

- keys are unique strings or numbers that identify an array item among the other array items
- keys help React to match array items with their corresponding components
  This is useful in complex code, i.e.: in case we have to/want to re-order items like the sorting feature in searches.
  Without indices react will not know what to update in the DOM.

##### Best Practices for Using Keys

- use keys that are included in the data i.e.: use the ID as key when we get the data from the back-end server
- if the app used locally and the data is generated locally &rArr; we can use the **randomUUID** method of the Crypto interface
  :warning: do not use this directly when rendering lists

Code example to include keys:

```javascript
export const GroceryList = () => {
  return (
    <ul>
      {groceries.map((item) => (
        <li key={item.id}>{item}</li>
      ))}
    </ul>
  );
};
```

> :exclamation: `key` is not a prop, to use the ID in a component as a prop we have to pass it separately

:bangbang: REMEMBER

- keep the keys the same (no randomUUID for list items because they change at each render - only good for initial dataset)
- keys must be unique (all items in the same array must be unique - similar keys in different arrays are ok)
- don't generate keys while rendering (React might not be able to match keys between renderings &rArr; lost user input, wrong item gets rendered, slow down due to all components being re-rendered not just those which where changed)

### 09. Handling Events

#### Handling Clicks

- we can use the **onClick** attribute on any element
- inside the attribute we define a callback function &rArr; triggered on click

```javascript
import { useState } from "react";

export const DrinkButtons = () => {
  const [drinkSelected, setDrinkSelected] = useState(false);

  const clickHandler = () => {
    setDrinkSelected(true);
  };

  return (
    <>
      <div className="Button-group">
        <button className="Button" onClick={clickHandler}>
          Tea
        </button>
      </div>
    </>
  );
};
```

#### Calling an Inline Function and setState Directly

we can define an anonymous function in place if we only want to change the state within the event handler function

```javascript
//the code before this is the same as the example above(Handling Clicks)
//execpt for that 'clickHandler' function is not defined
<button className="Button" onClick={() => setDrinkSelected(true)}>
  Tea
</button>
//the code after this is the same as the example above
```

#### Handling Text Input

- we can use the **onChange** atrribute for text input fields  
  (works for any input form fields i.e. text , radio b., checkbox, dropdown etc.)
- as with `onClick` we pass or define a callback function to handle the event change
- we can also use the **onKeyDown** attribute, which is triggered on key press

```javascript
import { useState } from "react";

export const GreetHeader = () => {
  const [name, setName] = useState("Kumiko!");

  const handleChange = (event) => setName(event.target.value); //retrieves the input value (event.target.value) and sets it as state

  return (
    <div>
      <h1>Hello {name}!</h1>
      <input type="text" value={name} onKeyDown={handleChange} />
    </div>
  );
};
```

[[winc-academy-notes.front-end-course]]  
[[winc-academy-notes.front-end-course.12_react-advanced]]
