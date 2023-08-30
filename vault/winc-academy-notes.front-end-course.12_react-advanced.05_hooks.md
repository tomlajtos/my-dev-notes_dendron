---
id: 1lcv3431ykvk915b9wx62n8
title: 05. Hooks
desc: "course: FE > module: React Advanced > topic: Hooks"
updated: 1693386753359
created: 1693384509387
---

<details>

  <summary>Toggle section links</summary>

[[05.01. Built-in Hooks|winc-academy-notes.front-end-course.12_react-advanced.05_hooks#0501-built-in-hooks]]  
[[05.01.01. useState (lets a component "remember" information)|winc-academy-notes.front-end-course.12_react-advanced.05_hooks#050101-usestate-lets-a-component-remember-information]]  
[[05.01.02. useRef (lets us reference a value that’s not needed for rendering)|winc-academy-notes.front-end-course.12_react-advanced.05_hooks#050102-useref-lets-us-reference-a-value-thats-not-needed-for-rendering]]  
[[05.01.03. useEffect|winc-academy-notes.front-end-course.12_react-advanced.05_hooks#050103-useeffect]]  
[[05.02. Custom Hooks|winc-academy-notes.front-end-course.12_react-advanced.05_hooks#0502-custom-hooks]]  
[[05.02. Exercise: Create a custom hook to store values in the browser's local storage|winc-academy-notes.front-end-course.12_react-advanced.05_hooks#exercise-create-a-custom-hook-to-store-values-in-the-browsers-local-storage]]  
[[05.03. useReducer hook|winc-academy-notes.front-end-course.12_react-advanced.05_hooks#0503-usereducer-hook]]--
[[05.03. Practical EXAMPLE for a reducer - shopping cart with several actions:|winc-academy-notes.front-end-course.12_react-advanced.05_hooks#practical-example-for-a-reducer---shopping-cart-with-several-actions]]  
[[05.03. Practical example for useReducer - Fetching Data|winc-academy-notes.front-end-course.12_react-advanced.05_hooks#practical-example-for-usereducer---fetching-data]]

</details>

[**Documentation**](https://react.dev/reference/react)

Hooks let us use different React features from our components. We can use built-in hooks or we can combine them to make custom ones.
Hooks are - constrained - functions:

- they must start with "use"
- they must be used at the top level of a functional component or inside other - custom - hooks
- they cannot be used outside a component or inside an if/else block or loop (React expects the hooks to be in the same order at each render)
- they cannot be used inside class components

## 05.01. Built-in Hooks

### 05.01.01. useState (lets a component "remember" information)

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

### 05.01.02. useRef (lets us reference a value that’s not needed for rendering)

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

### 05.01.03. useEffect

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

## 05.02. Custom Hooks

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

### Exercise: Create a custom hook to store values in the browser's local storage

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

## 05.03. useReducer hook

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

### Practical EXAMPLE for a reducer - shopping cart with several actions:

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
  switch(action.type) { // `break` is not needed since each case returns
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

### Practical example for useReducer - Fetching Data

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

| [[\ FE notes \| winc-academy-notes.front-end-course]] | [[\ previous \| winc-academy-notes.front-end-course.12_react-advanced.04_context]] | [[\ next \| winc-academy-notes.front-end-course.12_react-advanced.06_state-management]] | [[\ Overview \|winc-academy-notes.front-end-course.12_react-advanced.05_hooks#overview]] |
| :---------------------------------------------------- | :--------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------: |
