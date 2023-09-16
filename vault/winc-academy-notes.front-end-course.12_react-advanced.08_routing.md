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
- it does not utilize `event.preventDefault` > it just lets thorough the put or post request\*
  > \* look into this as this is a bit vague
- we can also use the `useSubmit` hook to submit a form
- actions take an async function and can return a regular response or anything else
- its data is available via the `useActionData` hook

## Example code to see the concepts in practice (blog with different routes)

The basic app for our example looks like this:

```javascript
// main.jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import "./index.css";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
);

// App.jsx
import { useState } from "react";
import { PostDetail } from "./PostDetail";
import { PostList } from "./PostList";

const App = () => {
  const [selectedPost, setSelectedPost] = useState(null);

  return (
    <>
      <PostList setSelectedPost={setSelectedPost} />
      <PostDetail postId={selectedPost} />
    </>
  );
};

export default App;
```

> **Installing React Router**: `npm install react-router-dom`

### **Setting up a Router**

We have to create our router and configure with the desired routs as follows:

**React Router** has a variety of **helper functions** to create different kind of routers:  
i.e `createBrowserRouter`: `BrowserRouter` is the most common router. When the router is created,
we pass it to the `RouterProvider`.

```javascript
// main.jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import { createBrowserRouter, RouterProvider } from "react-router-dom";
import "./index.css";

// this router replicates the base App behavior
// if the user navigates to 'www.blog.com' > the App element is rendered
const router = createBrowserRouter([{ path: "/", element: <App /> }]);

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>,
);
```

### Adding Routes

Add two routes: 1, route for _PostList_; 2, route for _PostDetail_
Remove `<App />` since it is only tracking `seletctedPost` state, URL info will replace that

```javascript
// main.jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import { createBrowserRouter, RouterProvider } from "react-router-dom";
import "./index.css";

/* create the two routes to `PostList` and `PostDetail`
!!! NOTE: postId --> URL-parameter, syntax in path prop: `:param`,
 awailable inside the component - id of the selected component */

const router = createBrowserRouter([
  { path: "/", element: <PostList /> },
  { path: "/post/:postId", element: <PostDetail /> },
]);

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>,
);
```

| [[\ fe notes \| winc-academy-notes.front-end-course]] | [[\ previous \| winc-academy-notes.front-end-course.12_react-advanced.07_talking-to-an-api]] | [[\ next \| winc-academy-notes.front-end-course.12_react-advanced.09]] | [[\ overview \|winc-academy-notes.front-end-course.12_react-advanced.08_routing#overview]] |
| :---------------------------------------------------- | :------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------: | :----------------------------------------------------------------------------------------: |
