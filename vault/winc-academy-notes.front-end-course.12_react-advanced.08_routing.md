---
id: 0colgx5dx8tzvzj2yfd38sg
title: 08. Routing
desc: "course: FE > module: React Advanced > topic: Routing"
updated: 1694764931078
created: 1694764809783
---

## Overview

<details>

  <summary>Toggle section links</summary>

**TODO**: add section links

</details>

<br/>

Client side routing, with the help of the **React Router** library, is the industry
standard for creating a multi-page UX in a single page application (from the servers perspective).
**React Router** is made by [ Remix ](https://remix.run/docs/en/main).

- it covers all functionalities that are mostly needed
- it has many tools that make routing straightforward(ish)

> It has gone through many major version changes and some of the older ones
> are still used in production. The APIs for the major wersions are not always
> compatible with each other, which is important to keep in mind - good to be up to date
> on the available features in the release notes.

[React Router Docs](https://reactrouter.com/en/main)

## React Router API

**Concepts that are important for understanding React Router:**

### Route

- router is configured by providing it one or more routs
- each rout has uniquely identifiable properties --> ensures the right component tree is rendered i.e.:
  - path property containing the URL, may include a URL parameter (i.e. an id etc.)
  - element property, determines what JSX should be rendered when the path and the URL matches
- routs can be nested, we can provide children (sub-route) to a certain route (i.e. .../pets/dogs)
  in the parent component we can render the child with the `<Outlet/>` component.

### Link

- it is a way to navigate a certain rout, to link pages with each other

### Loader

- it is used for reading data
- it is basically an async function we can pass to our Route
- it is a good way to ensure that the data is loaded at the correct time
- it returns the data we need in our component
- inside the comonent we can use the **useLoaderData** hook to access the data

### Action

- it is used for mutating (writing) data, i.e. we can use it to submit a form
- actions are called whenever a _non-get_ request is submitted to our route
- it does not utilize `event.preventDefault` > it just lets thorough the put or post request\* \* look into this as this is a bit vague
- we can also use the `useSubmit` hook to submit a form
- actions take an async function and can return a regular response or anything else
- its data is available via the `useActionData` hook

| [[\ fe notes \| winc-academy-notes.front-end-course]] | [[\ previous \| winc-academy-notes.front-end-course.12_react-advanced.07_talking-to-an-api]] | [[\ next \| winc-academy-notes.front-end-course.12_react-advanced.09]] | [[\ overview \|winc-academy-notes.front-end-course.12_react-advanced.08_routing#overview]] |
| :---------------------------------------------------- | :------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------: | :----------------------------------------------------------------------------------------: |
