---
id: xlidt4nqfa4ai0e9rwzmysl
title: 06. State Management
desc: "course: FE > module: React Advanced > topic: State Management"
updated: 1693384774785
created: 1693384707581
---

<details>

  <summary>Toggle section links</summary>

</details>

This is just a brief touch upon the topic of state libraries.
Need and personal/team preference are the main considerations when it comes to choosing a state management library.

## Redux and Redux Toolkit

[Redux Documentation](https://redux.js.org/)  
[Redux Toolkit Documentation](https://redux-toolkit.js.org/)

- one of the most popular libraries to manage complex state
- it is not React specific (works with vanilla JS and other frameworks as well)
- it works with reducers similar to what we use in React, so it has reducers and actions
- the state is kept in the 'store'
- the store contains a dispatch function to dispatch our actions
- it is possible to combine several reducers into the store

> **The updater function has to be pure**, arrays and objects cannot be mutated, a deep copy should be returned
> Since object is a reference type > direct update does not change the reference. (React won't know if the object was updated)
> A new object means new reference > React will know that it needs to do a re-render

### Redux Toolkit

- Redux maintainers recommend to use Redux and Redux Toolkit together
- it provides a more definitive way for using Redux
- provides abstractions to reduce the amount of boilerplate code otherwise need to be written

**PROS**

- long time industry tested in large production codebases
- great dev tools
- many add-on packages
- functional style (makes code easier to understand)
- provides great abstractions for building a store

**CONS**

- lots of boilerplate is needed for Redux without the Toolkit
- some learning curve
- writing immutable code could be quite complicated when working with deep state objects

## MobX

[MobX Documentation](https://mobx.js.org/README.html)

This library also works independently from React, but it is common practice to use them together

Design objective: "to manage state independently of any UI abstractions" _([michael.codes - UI as an Afterthought](https://michel.codes/blogs/ui-as-an-afterthought))_  
**MobX** uses the **observable state** pattern.
It is a data structure to which we can subscribe, in other words, which we can _observe_.

We wrap our component in an observer (provided by the library in form of a _React bindings package_),
which is a Higher-Order Component (a **function** that takes a **component** and returns a **new component**).
It uses the **observable** _decorator_ or **observable** functions in order to track objects (and it's state).

**PROS**

- truly reactive, updated state will cause a re-render of components that are consuming the state
- no need for actions/reducers > we simply modify the state
- less code

**CONS**

- not as widely used as other libraries (i.e. Redux)
- inner workings are mostly hidden / to much abstraction makes it harder to understand (less code though)
- uses some new features of JS that are not supported in older browsers

| [[\ FE notes \| winc-academy-notes.front-end-course]] | [[\ previous \| winc-academy-notes.front-end-course.11_react-basics]] | [[\ next \| winc-academy-notes.front-end-course.12_react-advanced.07]] | [[\ Overview \|winc-academy-notes.front-end-course.12_react-advanced.06_state-management#overview]] |
| :---------------------------------------------------- | :-------------------------------------------------------------------: | :--------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------: |
