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

| [[\ FE notes \| winc-academy-notes.front-end-course]] | [[\ previous \| winc-academy-notes.front-end-course.11_react-basics]] | [[\ next \| winc-academy-notes.front-end-course.12_react-advanced.07]] | [[\ Overview \|winc-academy-notes.front-end-course.12_react-advanced.06_state-management#overview]] |
| :---------------------------------------------------- | :-------------------------------------------------------------------: | :--------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------: |
