---
id: qzms8czz3nisfgbzl2h8829
title: FE Module 12 - React Advanced
desc: "Winc FE Module 12"
updated: 1693153176698
created: 1692450149764
---

## Overview

<details>

  <summary>Toggle section links</summary>

[[01. Debugging & Error Boundaries|winc-academy-notes.front-end-course.12_react-advanced#01-debugging--error-boundaries]]  
 [[02. Forms|winc-academy-notes.front-end-course.12_react-advanced#02-forms]]  
 [[03. Component Composition|winc-academy-notes.front-end-course.12_react-advanced#03-component-composition]]  
 [[04. Context|winc-academy-notes.front-end-course.12_react-advanced#04-context]]  
 [[05. Hooks|winc-academy-notes.front-end-course.12_react-advanced#05-hooks]]

</details>

## 01. Debugging & Error Boundaries

### Errors and Warnings

**Warnings** do not necessary break the code, but can cause faulty behavior.
**Errors** will cause the code to stop running.

Use **React Dev Tools**: look at the component tree, states, props, list of hooks,
component renders,DOM updates + breakpoints for debugging.

- **Profiler** - inspect how components are being rendered, during app usage/interaction

### Error Boundaries

ErrorBoundary components are similar to chatch blocks in JS
thay catch errors in any of the children components.

- connot be written as a function component => only CLASS component
  (reason: dependence on getDerivedStateFromError and componentDidCatch lifecylcle-methods > no alt in func. comps.)
  - getDerivedStateFromError shows smth. (fallback) instead of the child comp. where the error has happend
  - componentDidCatch is used for logging the error to an error logging service

```javascript
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    //Update state so the next render will show the fallback UI.
    logErrorToMyService(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // We can render any custome fallback UI
      return <h1>The shit has hit the fan.</h1>;
    }

    return this.props.children;
  }
}
```

**Add it anywhere** in the component tree. **Wrap** the component(s) which is/are, or the children of which are expected to fail,
**in** the opening and closing tags of the **ErrorBoundary component** >> the **error will bubble up to the closest error boundary**.
The app will keep running > there will be no need to reload the entire page:

```javascript
const App = () => {
  return (
    <ErrorBoundary>
      <ChildComponent1 />
      <ChildComponent2 />
    </ErrorBoundary>
  );
};
```

### Testing React Components

Use a bundler compatible testing framework.
i.e: Vite > Vitest; Parcel, Webpack > Jest

:warning: the testing framework needs to emulate the browser (React produces a DOM tree)  
For this use: JSDOM (is a Javascript DOM emulator), React Testing Library (**RECOMMENDED**)

Resources:

- [ Jest ](https://jestjs.io/)
- [Vitest](https://vitest.dev/)
- [React Testing Library](https://testing-library.com/docs/react-testing-library/intro)

## 02. Forms

### HTML Form elements

i.e.: `<input>`,`<textarea>`,`<select>` etc.

- These elements manage their own state in the browser by default
- On form submission: browser makes a request and navigates to a new page by default
  **We can (and usually want) to intercept this in react** for things like: validate, transform the data before submission
  To do this we have to **CONTROL THE FORM COMPONENTS**

### Controlled vs. Uncontrolled Components

By default, form elements manage their own internal state.  
In order to **control them**:

- we have to create some state to keep track of their value (and provide the value as a prop to the element)
- we also have to create a change handler function to keep the state updated  
   (**chenge** events are triggered by the form fields when the user interacts with them  
   **submit** event is triggered when the form as a whole is submitted)  
  Example of simple controlled form:

```javascript
export const ControlledInputForm = () => {
  const [inputValue, setInputValue] = useState("");

  const handleChange = (event) => {
    setInputValue(event.target.value); //update the state of inputValue with the value of the DOM element
  };

  const handleSubmit = (event) => {
    event.preventDefault(); // prevents default HTML form behavior
    console.log(event); // logs the user input
  };

  // returned form
  return (
    <form onSubmit={handleSubmit}>
      <input type={"text"} value={inputValue} onChange={handleChange} />
      <button type={"submit"}>Submit</button>
    </form>
  );
};
```

this again allows us to handle the data: validate, transforme the data and make our own custom request
to save the data to a database in our backend.

### Form Libraries

To avoid writing a lot of boilerplate code it is recommended to use libraries to create forms in React:
i.e.:

- [ Formik ](https://formik.org/)
- [ React Hook Form ](https://react-hook-form.com/)
- [ RJSF ](https://github.com/rjsf-team/react-jsonschema-form)

## 03. Component Composition

### Prop Drilling

When a state is mannaged at the top level of component hierarchy, and it is passed down to the childern
components until it reaches the sub component which actually needs to access it.
**What** is passed down as props: 1, value 2, setValue;  
I.e.:

```javascript
export const ComponentOne = () => {
  const [valueOne, setValueOne] = useState(0);
  return (
  <>
      <h1>Component 1</h1>
      <ComponentTwo value={valueOne} setterFn={setValueOne} />
  </>
  );
};

const ComponentTwo = ({value, setterFn}) => {
  return (
  <>
      <h2>Component 2</h2>
      <ComponentThree value={value} setterFn={setterFn} />
  </>
  );
};

const ComponentThree = ({value, setterFn}) => {
  return (
  <>
      <h3>Component 3</h3>
      <ComponentThree value={value} setterFn={setterFn} />
      <ComponentFour setterFn={setterFn} />
  </>
  );
};

 const ComponentFour = ({setterFn}) => {
  return {
    <button onClick={()=> setterFn((s) => s + 1)}>
      Increase the count
    </button>
  };
};
```

:warning: **The problem with prop drilling:**

- clutters up the code in case of complex project (intermediate component do not need the props, still need to pass through > adds extra lines)  
  **Solution**: Component composition, React Context, state management libraries

### Composing Components

#### The "children" Prop

It contains everything that goes between the opening and closing tags of a component  
For this we have to move from self-closing components to components with opening and closing tags  
**The children prop** can be i.e.: simple text, HTML elements, or other React components  
We have to use `{props.children}` to display the children of a component:

```javascript
export const Button = ({ onClick, ...props }) => {
  return (
    <CButton colorScheme={"teal"} onClick={onClick} {...props}>
      {props.children}
    </CButton>
  );
};
```

**When we choose to render nested components we can also pass properties in those more deeply nested components directly. This is component composition.**

- this allows us to directly pass props to the exact component where we need them.
  The example from "Prop Drilling" can be re-written like this:

```javascript
const ComponentOne = () => {
  const [valueOne, setValueOne] = useState(0);
  return (
    <>
      <h1>Component 1</h1>
      <ComponentTwo>
        <ComponentThree>
          <ComponentFour setterFn={setValueOne} />
        </ComponentThree>
      </ComponentTwo>
    </>
  );
};

//...
```

**BUT** we will need to use children inside both ComponentTwo and ...Three:

```javascript
//...

const ComponentTwo = ({ children }) => {
  return (
    <>
      <h2>Component 2</h2>
      {children}
      {/* display whatever is between the opening and closing tag of ComponentTwo
       as it was passed down from ComponentOne */}
    </>
  );
};

const ComponentThree = ({ value, children }) => {
  return (
    <>
      <h3>Component 3</h3>
      <p>The value is: {value}</p>  {/*displays the value from ComponentOne*/}
      {children}
      {/* display whatever is between the opening and closing tag of ComponentThree
       as it was passed down from ComponentOne */}
    </>;
  );
};

/**
 * ComponentFour doesn't need children, but it receives the "setterFn" prop from ComponentOne
 */
const ComponentFour = ({setterFn}) => {
  retutrn (
    <button onClick={() => setterFn((s) => s + 1)}>
      Increase the count
    </button>
  );
};
```

## 04. Context

[**Documentation**](https://beta.reactjs.org/learn/passing-data-deeply-with-context)

Component composition is not allways a practical solution i.e.:

- if we have different components needing the same data across many different levels of the component tree in different sections of our app  
  this would require us to write the entire component tree in the App.jsx file > this will limit the use of proper composition deeper in the tree  
  (like an app with light/dark themes, or info connected to current user that is used in many places in the app)

**Context** is a React mechanism that prowides a good solution for this problem
WHEN TO USE: when we have states that we need to share in many different places in our app
(_component composition_ is best to use in case when states have to be shared within a limited number of components which are realtively close together)

### The Context API

#### Creating Context (createContext function from React)

```javascript
export const Mycontext = createContext(defaultValue);
```

- defautlValue:
  - it can be anything, a simple value or a complex object (i.e. obj for user settings)
  - it is not dynamic in any way

:warning: it is important to allways export the context since it is used in multiple places

#### Providing Context

Indicate the **scope** of the context

```javascript
// import the Context, it should be placed as close as possible to the components which need it.

<MyContext.Provider value={contextValue}>
  {/* All the components that need the context */}
</MyContext.Provider>
```

:heavy_exclamation_mark: :point_down:

- **context.Provider** is a React component (a component can be a property of an object)
- we can provide a **value prop**, contains a value that might be different from the _defaultValue_
- this value can be - and usally is - dynamic (it is a value that is kept in state)
- when the value changes all the components inside the _Provider_ that use the _context_ will be re-rendered

#### Using Context

We can use the context inside any of the components that live inside the Provider component

```javascript
import { useContext } from "react";
import { MyContext } from "./MyContext.js";

const ContextUser = () => {
  const contextValue = useContext(MyContext);
  return <p>{contextValue}</p>;
};
```

- use the useContext hook
- import the created context
- the value what useContext returns is the value we've passed in as prop to the MyContext.Provider component

#### Multiple Contexts

In case of a complex up it is better to have multiple complex, each holding values that belong together  
i.e.: context holding user settings and context holding the theme (this will make possible to nest providers)

```javascript
<ThemeContext.Provider value={themeValue}>
  <UserContext.Provider value={userValue}>
    {/* All components that need these contexts */}
  </UserContext.Provider>
</ThemeContext.Provider>
```

When using the context, we would have two useContext hooks:

```javascript
import { useContext } from "react";
import { UserContext } from "./UserContext.jsx";
import { ThemeContext } from "./ThemeContext";

const ContextConsumer = () => {
  return (
    <>
      <p>Welcome, {userValue.name}</p>
      <p>You are using the {themeValue.type} theme!</p>
    </>
  );
};
```

:heavy_exclamation_mark: If we have two contexts with the same type, but with different values
(i.e. object) and where one is nested in the other, \**React will allways use the context of the provider closest
to the component, *the provider that is nested the deepest\*.

#### Best Practices

1. **Setting a displayName**
   While debugging in React Dev Tools, the Provider component will generally appear as `<Context.Provider>`  
   If we set a display name like, `UserContext.displayName = "UserContext";` in Dev Tools
   we'll see `<UserContext.Provider>`

2. **Creating a custom Provider component**
   Good practice to create a new component that makes using Context cleaner.  
   We call these "custom Provider components"  
   Advantages:
   - they contain state and code to update the state
   - if a code for a context needs to be updated, we can do this by updating only the custom Provider
     and the components that use the context, there's no need to update intermediate components

```javascript
//UserContext.jsx
import { useState, createContext } from "react";

export const UserContext = createContext(null);

export const UserProvider = ({ children }) => {
  const [name, setName] = useState("Someone");
  const value = {
    state: { name },
    actions: { setName },
  };
  return <UserContext.Provider value={value}>{children}</UserContext.Provider>;
};
```

To provide the context:

- keep the state in this component
- provide the value which is contained in this state
- provide a setter to change the state value
- use the `children` prop to render whatever is in between the opening and closing tag of the Provider

```javascript
// App.jsx
import {UserProvider} from "./UserProvider.jsx";

const App () => {
  return (
  <>
      <h1>Welcome to our App</h1>
      <UserProvider>
        {/* The rest of the app comes here */}
      </UserProvider>
    </>
  )
}
```

**This will make sure that our App component is nice and clean when we use multiple contexts**.
It is better than keeping different kinds of context states inside the App component

3. **Creating a custom context hook**
   A custom context hook will make the process of:  
   importing useContext > importing Context > calling useContext nicer.

```javascript
import { useContext } from "react";
import { UserContext } from "./UserContext.jsx";

export const useUser = () => {
  const context = useContext(UserContext);
  if (!context) {
    throw new Error("useUser must be used within a UserProvider!");
  }
  return context;
};
```

This `useUser` hook (which is a function) returns the result of useContext(UserContext), which is the context.
We can use this custom hook instead of the useContext hook itself along with UserContext.

```javascript
import { useUser } from "./useUser.jsx";

const UserName = () => {
  const {
    state: { name },
  } = useUser();
  return <p>Hello, {name}</p>;
};
```

- we are getting the user name from the context value's state prop
- we can access the setName function from the context's action prop (in case we wanted to change the user's name in the component)
- if we use the UserName component outside the UserProvider, we will get the error message > and get a good hint why something doesn't work as expected

:bangbang: The custom Provider (contains the state management) and the custom Hook helps to separate business logic from UI and helps with debugging as well.

## 05. Hooks

[**Documentation**](https://react.dev/reference/react)

Hooks let us use different React features from our components. We can use built-in hooks or we can combine them to make custom ones.
Hooks are - constrained - functions:

- they must start with "use"
- they must be used at the top level of a functional component or inside other - custom - hooks
- they cannot be used outside a component or inside an if/else block or loop (React expects the hooks to be in the same order at each render)
- they cannot be used inside class components

### 05.01. Built-in Hooks

1. #### useState (lets a component "remember" information)

- It lets us add a state variable to our component
- and it helps us to keep track of state between renders of a functional component
- Its can keep simple values or complex ones like objects, arrays, even functions
- When we call the useState, we can provide an initial value
- It returns an array with exactly two values: the current state and the set-function

```javascript
const [value, setValue] = useState("initial value"); // crates a state value and initialize it to the string "initial value"
```

**To update the state** we can use the `setValue` function:

```javascript
setValue("new value");
// it can take a value of any type, but it can only take a pure function (updater)
// with the pending pending state as its only argument
// it has no return value
```

This will ensure that at the next render the value will be "new value", as React stores the next state  
updating the state will trigger a re-render of the component  
:exclamation: ** this trigger is what makes a hook a hook**
it hooks into React and lets us control when to re-render

:warning: calling the `set` function **will not change** the current state value in the already executing code  
`useState` will return the new value when (and starting from) the next render happens - _in other words it is not synchronous_  
i.e.:

```javascript
import { useState } from 'react';

export const CounterComponent() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
    console.log(count) // 0, not re-rendered yet
    setCount(count + 1);
    console.log(count) // 0, not re-rendered yet
  }

  return (
    <button onClick={handleClick}>
      You pressed me {count} times
      {/* clicking the button will only add 1 to the count despite the two set functions*/}
    </button>
  );
```

To overcome this **we can pass an updater function as the argument** to the set function

```javascript
import { useState } from 'react';

export const CounterComponent() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount((c) => c + 1); // it is convention to name the pending state var the first letter of the state var name
    console.log(count) // 0, not re-rendered yet
    setCount((c) => c + 1); // React will put the updater function in a queue and execute them in the same order during the next render
    console.log(count) // 0, not re-rendered yet
  }

  return (
    <button onClick={handleClick}>
      You pressed me {count} times
      {/* clicking the button will add 2 since during the next render React will call
      the updater function from the queue in order and updates the state with the final value */}
    </button>
  );
```

We can also **set the initial state with a function** if it is required (i.e. data has to be processed before it gets set as a value)

```javascript
const createInitialState = () => {
  // some expensive operation
};

const [value, setValue] = useState(createInitialState());
// the initial value is only set once, but the createInitalState
// function will be called at each render
// TO PREVENT this we should omit the invocation in place - no `()`
// ...
const [value, setValue] = useState(createInitialState); // this is only called once
```

**Reseting State**
We can do this by keeping the initial state in a separate constant and use a reset function
to set the state to the initialState variable

```javascript
const initialState = "initial value";
// ...
const handleStateReset = () => {
  setValue(initialState);
};

return (
  // htmx
  <button type={"button"} onClick={handleReset}>
    Reset
  </button>
  // htmx
);
```

This can get complicated with multiple state values  
For this purpose (besides besides rendering lists) we can **use the key attribute** to reset the component's state.
If we pass our component a new key, it will be re-mounted and as the result a new version of the component will be rendered.

```javascript
// example from (https://react.dev/reference/react/useState)
import { useState } from "react";

export default function App() {
  const [version, setVersion] = useState(0);

  function handleReset() {
    setVersion(version + 1);
  }

  return (
    <>
      <button onClick={handleReset}>Reset</button>
      <Form key={version} />
    </>
  );
}

function Form() {
  const [name, setName] = useState("Taylor");

  return (
    <>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <p>Hello, {name}.</p>
    </>
  );
}
```

2. #### useRef (lets us reference a value that’s not needed for rendering)
   > "_Refs are an escape hatch that should be used sparingly._"
   > [documentation](https://react.dev/reference/react/useRef#usage)

It is perfect for storing info that does not effect the visual output of the component.
(in other words the ref value we want to keep track of does not effect the rendering logic of the component)

- we can store info between re-renders (regular variables reset on every render).
- changing a ref does not trigger a re-render (changing state will trigger a re-render).
- The information is local to each copy of our component (unlike the variables outside, which are shared).

Example code:

```javascript
import { useRef } from "react";

const Component = () => {
  const ref = useRef(0);
  const handleClick = () => {
    console.log(ref.current); // 0
  };
  return <button onClick={handleClick}>Click me</button>;
};
```

- useRef will return a simple javascript object
- it only has one property: `.current`
- the initial value (of type any) for the `.currant` prop will be ignored after the initial render
- to update the ref value, we need assign a new value to its `.current` prop manually
- it will preserve the value between re-renders
- :warning: we should not store info that needs to be displayed in a `Ref`

```javascript
import { useRef } from "react";

const Component = () => {
  const ref = useRef(0);
  const handleClick = () => {
    ref.current = 1; // assign a new value to the .current property, directly
    console.log(ref.current); // 1
  };
  return <button onClick={handleClick}>Click me</button>;
};
```

:warning:  
We should only assign/read the value of the ref inside event handlers
or inside effects and never render the value of refs or access it in other way at the top
level of our component.

Example of useRef implementation to reference a value - _a timer component_:

- wa can use `setTimeout` (js-built-in) to i.e. 10 to show an alert.
- cancel the timer with `clearTimeout` (js-built-in)

**Justification for useRef**:  
The callback function (which runs the end of timer alert after defined time) of the timeout
is put on the eventloop and sice the component could be re-rendered during the time set in
setTimeout > we would loose the value it is set to during the re-render **IF** we'd store
the value in the component.

```javascript
/**
 * Example for the wrong code where the value is lost upon re-render:
 * if re-render happens while the timer is running the timerId will become `undefined` again
 */
const Timer = () => {
  let timerId;
  const startTimer = () => {
    timerId = setTimeout(() => alert("Done!"), 10000);
  };
  const cancel = () => {
    clearTimeout(timerId);
  };

  return (
    <>
      <button onClick={startTimer}>Start Timer</button>
      <button onClick={cancel}>Cancel Timer</button>
    </>
  );
};
```

The solution is that we use a ref:

```javascript
import { useRef } from "react";

const Timer = () => {
  const timerRef = useRef(null);

  const startTimer = () => {
    clearTimeout(timerRef.current); // clear out previously set timeout, to avoid having 2 or more timers running on render
    timerRef.current = setTimeout(() => alert("Done!"), 10000);
  };
  const cancel = () => {
    clearTimeout(timerRef.current);
  };

  return (
    <>
      <button onClick={startTimer}>Start Timer</button>
      <button onClick={cancel}>Cancel Timer</button>
    </>
  );
};
```

**Manipulating the DOM with a ref**  
React has built-in support for DOM manipulation, since it is very common practice.--
We can refer to a DOM element to perform some operations on them (outside of React's rendering logic)  
i.e.: attach a ref to an element and focus it, or scroll it into view.--
It requires useEffect to use it to its full potential, but that comes later in the material.
Example:

```javascript
// Form component

import { useRef } from "react";

export const Form = () => {
  const inputRef = useRef(null); // declare a ref object
  const handleClick = () => {
    console.log(inputRef.current); // logs the input element
    inputRef.current.focus(); // focus the input element with the .focus() method
  };

  return (
    <>
      {/* Attach a ref to the input elemetn by passing it as a prop
      React.createElement will return an HTML element which will be assigned
      to inputRef.current */}
      <input ref={inputRef} />
      <button onClick={handleClick}>Focus the input</button>
    </>
  );
};
```

**forwardRef** API  
:exclamation: we cannot pas a ref to our component in React, but only to an HTML element in JSX  
By default, we will get null. We'll also get an error + hint from React. Because React does not allow
a component to access the DOM nodes of other components. **Not even for its children components**.
Reason: it introduces instability to the code.

**THERE IS A WORKAROUND**, but it is usually not a good idea to let a component alter the DOM
elements of another component.

To do this we have to use `forwardRef`, the component has to opt in to the behavior of exposing DOM nodes

```javascript
import {forwardRef} from "react"

const ExampleInput = forwardRef((props,ref)) => {
  return <input {...props} ref={ref} />;
});
```

- when a component receives a ref argument > that tell React to put the corresponding DOM node in the inputRef.current
- `ExampleImput` is declared using `forwardRef`
- in this case (see below) the component receives the `inputRef` from `Form` and passes that ref to its `<input>` child.

```javascript
//Form component

import { forwardRef, useRef } from "react";
import { ExampleImput } from "./ExampleImput.jsx";

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <ExampleInput ref={inputRef} />
      <button onClick={handleClick}>Focus the input</button>
    </>
  );
}
```

> :warning: [**Best practices for using refs**](https://react.dev/learn/referencing-values-with-refs#best-practices-for-refs)

Resources to concepts/tools used in coresponding exercises:  
[ scrollIntoView() method ](https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollIntoView)  
[ scroll usecases (recommended blog from the module)](https://codefrontend.com/scroll-to-element-in-react/)

3. #### useEffect

[**documentation**](https://react.dev/learn/synchronizing-with-effects)  
[**when not to use effects**](https://react.dev/learn/you-might-not-need-an-effect)

Effects provide a way **for a component to synchronize with external systems** i.e. setting up server connections,
sending analytics log after a component is rendered, syncing third-party widgets etc.
**Effects let us run code after a component is rendered**  
They let us **specify side effects that are caused by rendering** and not by an event. (i.e. server connection setup)

**Syntax**

```javascript
useEffect(setupFunction, dependencies);
```

- setupFunction: runs when to component is rendered to the DOM, and runs again upon dependency change
- cleanupFunction: culd be returned by the setupFunction, preforms actions to remove a component (unmount),
  or actions that are necessary before the effect runs again.
- dependencies parameter: optional, it is an array, if not present the setup function runs on each render,
  if present, the setup function will only run when the dependencies change. If it is an empty array, the setup
  function will run only once at the first render.
- useEffect does not return anyting, exept for a cleanup function, so it is never assigned to a variable.

_examples_:

1. ChatRoom Component, Effect has a dependecy array with a dependency (roomId)

```javascript
import { useEffect } from "react";

export const ChatRoom = ({ roomId }) => {
  useEffect(() => {
    const connection = createConnection(roomId);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]); // the setup function will only run when roomId changes, and at initial render

  //...
};
```

The cleanupFunction runs whenever the component is unmounted, to disconnect from the current chat room.
In case of roomId change the cleanup function of the old version of the component runs and THEN the new
setupFunction of the new version of the component runs, to establish connection to the room with the new ID.

2. Fetching Data, empty array as dependency
   > :information*source: \_in complicated apps it is often better to rely on a library for data fetching*
   > (Async code in react will be discussed in a later part of the material.)

```javascript
useEffect(() => {
  async function requestShoppingList() {
    const json = await fetchShoppingList();
    // if (!ignore) { // when fetching i.e. user specific data and we have an ID, as a dependency. Ignore is set by the cleanupF.
    setShoppingList(json);
    // }
  }
  requestShoppingList();

  /**
   * cleanup function (not necessary in this exemple > emty dependency array)
   *
   * return () => {
   *   ignore = true;
   * }
   */
}, []);
```

Here we make a server request for data with Effect and set it to local state.--
The empty dependency array will make sure that that it is only called once,
on component mount (prep before first render) and not during re-renders.

> [!IMPORTANT] > _it needs a cleanup function which either aborts or ignores the fetch IF we fetch something specific to_

Although, it is a popular way to fetch data with Effects, it could be cumbersome with many drawbacks* and doing this directly
in Effects should be done if there is no framework (integrated data fetching mechanism) option available,
or if building a client side cash is not possible either.
*i.e.:

- Effects do not run on the server (initial server rendered HTML will only have loading state, no data)
- Fetching directly in Effects can lead to "network waterfalls"
- There is no preload or cashing of data, usually
- usually not ergonomic code (necessary to avoid race conditions - see later)

3. Changing the document title - no dependency is used in this example

```javascript
import { useEffect, useState } from "react";

export const TitleChange = () => {
  const [title, setTitle] = useState("Title");
  const [toggle, setToggle] = useState("off");

  useEffect(() => {
    console.log("setting the title!");
    document.title = title;
  });

  return (
    <>
      <input
        type={"text"}
        value={title}
        onChange={(e) => setTitle(e.target.value)}
      />
      <button onClick={() => setToggle(toggle === "on" ? "off" : "on")}>
        Toggle
      </button>
      {toggle}
    </>
  );
};
```

Typing anything in the text input will cause a re-render so as clicking the "Toggle" button.
The Effect will run on ANY state change and re-render in consequence.
**Best practice is to specify when an Effect needs to run**
In this case, the title should be a dependency of the Effect.

**Summary table**

|                                     | setup function |  setup f.<br>+<br>cleanup f.   |       setup f.<br>+<br>dependencies        |                    setup f.<br>+<br>cleanup f.<br>+<br>dependencies                    | setup f.<br>+<br> cleanup f.<br>+<br>empty dep.arr. |
| :---------------------------------- | :------------: | :----------------------------: | :----------------------------------------: | :------------------------------------------------------------------------------------: | :-------------------------------------------------: |
| component is<br>added to DOM        |   setup runs   |           setup runs           |                 setup runs                 |                                       setup runs                                       |                setup runs <tr></tr>                 |
| component is<br>re-rendered         |   setup runs   | - cleanup runs<br>- setup runs | IF dependencies have changed<br>setup runs | - IF no dependency change<br>THEN do nothing<br>ELSE<br>- cleanup runs<br>- setup runs |                do nothing <tr></tr>                 |
| component is<br>removed<br>from DOM |      ---       |          cleanup runs          |                    ---                     |                                      cleanup runs                                      |                    cleanup runs                     |

---

### 05.02. Custom Hooks

[Custom Hooks documentation](https://reactjs.org/docs/hooks-custom.html)

We can use custom hooks to share (stateful) logic between components and avoid duplicate code.
It is possible to build our own hooks, aka. custom hooks. We could use them for different purposes like:

- fetching data
- connecting to a service (chat room)
- keep track if users are online
  etc.

> [!IMPORTANT]
> If our functionality calls React built-in hooks and we'd like to extract that to be used globally in our app, we should create a custom hook.
> If it does not call a hook we should just extract it into a regular utility function
> A hook's name should always start with `use`
> REMINDER: hooks cannot be called conditionally

Example:

```javascript
import { useState } from "react";

export const useToggle = (initialState = false) => {
  const [toggleState, setToggleState] = useState(initialState);

  const toggle = () => {
    setToggleState(!toggleState);
  };
  return [toggleState, toggle];
};
```

This is how we can create a useToggle hook. It creates a state that we can change back and forth between false and true.
It is very simmilar to a useState hook but with a **custom setter function** (toggle).
In a component where it is used we can only access what it returns (toggleState, toggle function)
**A stateful logic is shared, but not the state**. i.e.: we can not set the toggle state using setToggleState.
** This is called encapsulation** (of the logic) > helps to prevent bugs.
We can make this even more sophisticated:

```javascript
export const useToggle = (initialState = "off") => {
  if (initialState !== "off" && initialState !== "on") {
    trow new Error(
      "useToggle must ve initialized with either 'on' or 'off'"
    );
  }
  const [toggleState, setToggleState] = useState(initialState);

  const toggle =()=> {
    setToggleState(toggleState === "on" ? "off" : "on");
  };
  return [toggleState, toggle];
}
```

#### Exercise: Create a custom hook to store values in the browser's local storage

[Doc: Web Storage API on MDN](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API)
[Doc: localStorage on MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)

- this built-in browser API, helps to save values between sessions
- values can be restored IF we use the same browser on the same computer (obvi)
  We'll use the `window.localStorage` object which have several methods for this usecase i.e.:
- `getItem()`, `setItem()`
- for this we need a **key** ('name' in the exercise) to identify our data

```javascript
window.localStorage.setItem("key", value);
window.localStorage.getItem("key");
```

### 05.03. useReducer hook

This React hook lets us add a reducer (function) to our component, and helps us to manage state inside a component.
A reducer is a pure function outside the component logic that specifies how the state should be updated.
It originates from the concept of Array.reduce() method, but instead of usin previous and current values to return a final value,
it **calculates the next state based on the previous state and the current action** :

```javascript
const reducer = (state, action) => nextState;

const Comp = () => {
  const [state, dispatch] = useReducer(reducer, initialState);
};
```

**useReducer** returns the **current state** and a **dispatch function** >> in an array (like useState)
The **dispatch function** indirectly updates the state through the reducer function >> triggers a **re-render**
This update happens as the result of user interaction.

We **call the dispatch function** with an argument that represents the user action, it is called _action_ >> this is the
only argument to the dispatch function. The _action_ will be **provided to the reducer function** as its second argument
(first is the current state) **via the dispatch function**.

> ** Whenever an 'action dispatch' happens useReducer will run the reducer function to update the state**.
> The **new state** is the **result** of calling the **reducer function**.
> The reducer should **never mutate** the current state, but should return a new state insted.

An example for a dispatch function would look like i.e.:

```javascript
const handleClick = () => {
  dispatch({ type: "updated_user" });
  // ...
};
```

The industry consensus on how an _action_ object should look like is i.e. the following:

- it could be any type, but by convention it is an object (usually)
- with a type property for identification
- also other, optional, properties for more info

```javascript
const action = {
  type: "updated_user",
  firstName: "Penny",
  // ...
};
```

- the _dispatch function_ has no return value
- it only updates the state for the next render
- re-render only happens if the provided value is different from the current state (Object.is comparison)

To avoid spelling errors action types can be assigned to constants  
(usecase: i.e. when dispatching actions from many different files)

```javascript
const UPDATED_USER = "updated_user";
const action = {
  type: UPDATED_USER,
  // ...
};
```

#### Practical EXAMPLE for a reducer - shopping cart with several actions:

Write _actions_ to handle items

```javascript
/**
* Action examples
*/
const addItemAction = {
    type: ‘added_item’,
  // name and quantity is determined by the user
    name: ‘broccoli’,
    quantity: 1
};

// for the next two action we only pass-in 'itemId'
// no need for other info
const removeItemAction = {
    type: ‘removed_item’,
    itemId: 1
};

const checkOffItemAction = {
    type: ‘checked_off_item’,
    itemId: 2
};

/**
* State example
* (state arg., or next state returned by the reducer)
*/
const shoppingListState = [
  {
    name: "broccoli",
    quantity: 1,
    checked: false,
    itemIs: 1,
  },
  {
    name: "apples",
    quantity: 5,
    checked: false,
    itemIs: 2,
  },
  ...
];


/**
* Reducer:
* (i.e. shoppingListReducer.js)
* use a switch statement inside (useful with high number of actions)
*/
export const shoppingListReducer = (state, action) => {
  switch(action, type) { // `break` is not needed since each case returns
    case "added_item": {
      const lastItem = state.at(-1);
      const newItemId = lastItem ? lastItem.itemId + 1 : 1
      const item = {
        name: action.name,
        quantity: action.quantity,
        checked: false,
        itemId: newItemId
      };
      return [...state, item]; // return a new state objecy with `...`, to keep the function pure
      // spreading the state results a shallow copy only, better to use a deep clone
      // if the obj is more complex [https://developer.mozilla.org/en-US/docs/Glossary/Deep_copy]
    }
    case "removed_item":
      // filter out the element with `itemId` matching the `itemId` in the action
      return state.filter((item) => item.itemId !== action.itemId);
    case "checked_off_item":
      // find and copy items with matching `itemId` set `checked: true` in the copy
      return state.map((item) => {
        if(item.itmeId === action.itemId) {
          return {
            ...item,
            checked: true
          }
        };
        return item;
      });
    default:
      return state;
  }
}
```

:warning: **REMINDER: keep reducers pure**  
If a current state is mutated, it is likely that components that are using the state will not recognize
that it's been changed and will render inacurate information.

#### Practical example for useReducer - Fetching Data

```javascript
/**
 * Create a Shopping list component with useReducer
 * i.e. ShoppingList.jsx
 */

import { useReducer } from "react";
import { shoppingListReducer } from "./shoppingListReducer";

export const ShoppingList = () => {
  const [state, dispatch] = useReducer(shoppingListReducer, [
    {
      // initial state (the first item in the list)
      name: "broccoli",
      quantity: 1,
      chacked: false,
      itemId: 1,
    },
  ]);

  // dispatch function for "added_item" action
  const addItem = (event) => {
    event.preventDefault();
    dispatch({
      // action obj as arg for dispatch
      type: "added_item",
      name: event.target.elements.name.value,
      quantity: Number(event.target.elements.quantity.value),
    });
  };

  // dispatch function for "checked_off_item" action
  const checkItem = (itemId) => {
    dispatch({
      type: "checked_off_item",
      itemId,
    });
  };

  // dispatch function for "removed_item" action
  const removeItem = (itemId) => {
    dispatch({
      type: "removed_item",
      itemId,
    });
  };

  return (
    <>
      <h1>My Shopping List</h1>

      {/* Add a form to make it interactive */}
      <form onSubmit={addItem}>
        <label htmlFor={"name"}>name:</label>
        <input type={"text"} name={"name"} />
        <label htmlFor={"quantity"}>quantity:</label>
        <input type={"number"} name={"quantity"} step={1} min={1} />
        <button type={"submit"}>Add Item</button>
      </form>

      <ul>
        {/* Render list items */}
        {state.map((item) => (
          <li key={item.itemId}>
            {/* add checkbox to toggle checked status */}
            <input
              type={"checkbox"}
              checked={item.checked}
              onChange={() => checkItem(item.itemId)}
            />
            {item.quantity} * {item.name}
            {/* add delete button */}
            <button type={"button"} onClick={() => removeItem(item.itemId)}>
              X
            </button>
          </li>
        ))}
      </ul>
    </>
  );
};
```

| [[\ FE notes \| winc-academy-notes.front-end-course]] | [[\ previous \| winc-academy-notes.front-end-course.11_react-basics]] | _end-of-course_ | [[\ Overview \|winc-academy-notes.front-end-course.12_react-advanced#overview]] |
| :---------------------------------------------------- | :-------------------------------------------------------------------: | :-------------: | :-----------------------------------------------------------------------------: |
