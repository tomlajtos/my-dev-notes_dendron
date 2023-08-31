---
id: 85ukw3jk9bpp24sh0867t11
title: 04. Context
desc: "course: FE > module: React Advanced > topic: Context"
updated: 1693384658696
created: 1693384478409
---

## Overview

<details>

  <summary>Toggle section links</summary>

[[The Context API|winc-academy-notes.front-end-course.12_react-advanced.04_context#the-context-api]]  
[[Creating Context (createContext function from React)|winc-academy-notes.front-end-course.12_react-advanced.04_context#creating-context-createcontext-function-from-react]]  
[[Providing Context|winc-academy-notes.front-end-course.12_react-advanced.04_context#providing-context]]  
[[Using Context|winc-academy-notes.front-end-course.12_react-advanced.04_context#using-context]]  
[[Multiple Contexts|winc-academy-notes.front-end-course.12_react-advanced.04_context#multiple-contexts]]  
[[Best Practices|winc-academy-notes.front-end-course.12_react-advanced.04_context#best-practices]]

</details>

[**Documentation**](https://beta.reactjs.org/learn/passing-data-deeply-with-context)

Component composition is not allways a practical solution i.e.:

- if we have different components needing the same data across many different levels of the component tree in different sections of our app  
  this would require us to write the entire component tree in the App.jsx file > this will limit the use of proper composition deeper in the tree  
  (like an app with light/dark themes, or info connected to current user that is used in many places in the app)

**Context** is a React mechanism that prowides a good solution for this problem
WHEN TO USE: when we have states that we need to share in many different places in our app
(_component composition_ is best to use in case when states have to be shared within a limited number of components which are realtively close together)

## The Context API

### Creating Context (createContext function from React)

```javascript
export const Mycontext = createContext(defaultValue);
```

- defautlValue:
  - it can be anything, a simple value or a complex object (i.e. obj for user settings)
  - it is not dynamic in any way

:warning: it is important to allways export the context since it is used in multiple places

### Providing Context

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

### Using Context

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

### Multiple Contexts

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

### Best Practices

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

| [[\ FE notes \| winc-academy-notes.front-end-course]] | [[\ previous \| winc-academy-notes.front-end-course.12_react-advanced.03_component_composition]] | [[\ next \| winc-academy-notes.front-end-course.12_react-advanced.05_hooks]] | [[\ Overview \|winc-academy-notes.front-end-course.12_react-advanced.04_context#overview]] |
| :---------------------------------------------------- | :----------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------: | :----------------------------------------------------------------------------------------: |
