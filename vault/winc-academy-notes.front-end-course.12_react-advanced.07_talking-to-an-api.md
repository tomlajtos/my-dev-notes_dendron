---
id: qje9ge7pz8kjkocfax6sids
title: 07. Talking to an API
desc: "course: FE > module: React Advanced > topic: Talking to an API"
updated: 1693581669629
created: 1693578489006
---

## Overview

<details>

  <summary>Toggle section links</summary>

</details>

<br/>

**Related:** [[Module 10 - Talking to the Back-end|winc-academy-notes.front-end-course.10_talking-to-the-back-end]]

In React it is important to know that at what point and in which component we should make/trigger our request.
We also have to be careful with component rendering > so we would not overload the server we're making the request to

## Requesting data and keeping it in state

Fetching data from the back-end and or from an external service is one of the most common task for a web-app.
A good option to handle such task is to use the [[useEffect|winc-academy-notes.front-end-course.12_react-advanced.05_hooks#050103-useeffect]] hook.

**Example** - social media app - show a list of friends

- we have to make the request before we can render anything (async - Module 10)
- we have to nest an async function within the useEffect (we cant use async directly in useEffect,  
  async returns a promise and useEffect can only return a cleanup function, nothing else)
- we have to call the nested async function from the useEffect hook

```javascript
useEffect(() => {
  async function fetchData() {
    const response = await fetch(
      "https://www.my-antisocial-network.com/api/friends",
    );
    const json = await response.json(); // convert response to JSON
    console.log(json);
  }
  fetchData();
  // empty dep. array ensueres that the effect only runs once >> request is sent only once
  // when the component is first loaded
}, []);
```

### Keeping the data state

**Same example as before - extended**

- since we're fetching data inside a component, it is logical to keep it in the local state  
  (if fetched data is used in other components too, it is better to use a state management solution)
- since we have the data saved in the component's state we can render the `friendsList` value

```javascript
const [friendsList, setFriendsList] = useState([]);

useEffect(() => {
  async function fetchData() {
    const response = await fetch(
      "https://www.my-antisocial-network.com/api/friends",
    );
    const json = await response.json(); // convert response to JSON
    setFriendsList(friends);
  }
  fetchData();
});

return (
  <div className="App">
    <h1>Friends</h1>
    <ul>
      {friendsList.map((friend) => (
        <li key={friend.id}>{friend.name}</li>
      ))}
    </ul>
  </div>
);
```

## Sending new requests after changing a prop

Since requests are asynchronous the order of requests (if multiple were made) could be different than the order
of responses we receive for them. We have to make sure that irrelevant fetch requests are ignored. _(It is not possible to
to undo a network request, so the response from an older request could overwrite the data of the current request if
we do nothing about it.)_

> We can use an **ignore flag** to make sure that outdated responses will not be shown.
> This flag is set in the cleanup function.

```javascript
const [posts, setPosts] = useState([]);

useEffect(() => {
  let ignore = false;
  setPosts([]); // reset the state

  async function fetchData() {
    const response = await fetch(
      `http://www.my-antisocial-network.com/api/friends/${friendId}/posts`,
    );
    const posts = await response.json();
    if (!ignore) {
      setPosts(posts);
    }
  }
  fetchData();

  // when the friendId changes the cleanup function runs first and sets
  // ignore to 'true' > if a response comes back after this it will be ignored
  return () => {
    ignore = true;
  };
}, [friendId]);
```

We also have to **reset the state holding the data** each time the corresponding prop value changes.
See line `setPost([]);` in the example code above. If the request takes long we will not show
stale data (i.e. posts from the prev user) in the app.

> Caching make sense when the user navigates frequently around the app. It will reduce the amount of
> request we'd need to make. Fetching libraries usually include mechanisms for caching as well
> (could be complicate it to implement it on our own; _comes later in the module material_).

## Sending a create or update request

> A common mistake when it comes to handling requests for updating data or adding data is to put everything in
> local state and send a `POST` or `PATCH` request with useEffect when the user submits or changes a value.
>
> **This should be done within the event handlers since they have all the necessary data.**

**Example to show how this should be done** (liking a post in the demo social app):

```javascript
const likePost = (postId) => {
  // POST request, the expected format of the request body is determined by the server
  fetch("https://my-antisocial-network.com/api/like", {
    method: "POST",
    body: JSON.stringify({
      postId,
      userId: myUserId,
    }),
  });
};

return (
  <>
    <h1>Posts</h1>
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          <h2>{post.title}</h2>
          <p>{post.body}</p>
          <button onClick={() => likePost(post.id)}>Like!</button>
        </li>
      ))}
    </ul>
  </>
);
```

As for logic to store and show the status of our likes let's assume the following:

- each post object has a '_likedBy_' property
- this prop contains an array of userId-s of those who liked the post
- when we like a post our userId shuld be added to this array and so on

```javascript
/**
 * previos example, extended with the logic for likes,
 * check if the response is ok > add myUserId to likedBy prop of post
 */
const likePost = (postId) => {
  // POST request, the expected format of the request body is
  // determined by the server
  fetch("https://my-antisocial-network.com/api/like", {
    method: "POST",
    body: JSON.stringify({
      postId,
      userId: myUserId,
    }),
  });

  // add myUserId to the likedBy prop a of the matching post
  if (like.ok) {
    setPosts((posts) =>
      posts.map((post) => {
        if (post.id === postId) {
          post.likedBy = [...post.likedBy, myUserId];
        }
        return post;
      }),
    );
  }
};

// ...

// IF myUserId is in the likedBy array THEN show Unlike button ELSE show the
// Like button | here we assume that there is an unlikePost function as well
return (
  <>
    <h1>Posts</h1>
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          <h2>{post.title}</h2>
          <p>{post.body}</p>
          {post.likedBy.includes(myUserId) ? (
            <button onClick={() => likePost(post.id)}>Unlike!</button>
          ) : (
            <button onClick={() => likePost(post.id)}>Like!</button>
          )}
        </li>
      ))}
    </ul>
  </>
);
```

PATCH, PUT and DELETE methods in React are handled the same way as the fetch example above.
**Only the GET request (reading) is done inside an effect.**

> **Exception**: i.e. a POST for a logging service (track page hits etc.), since _it does not originate from a user action_
> Simple rule: if the request is the result of a user action **use an event handler**, if it should be the result
> of rendering a component, **use the useEffect hook** like with a GET request.

| [[\ FE notes \| winc-academy-notes.front-end-course]] | [[\ previous \| winc-academy-notes.front-end-course.12_react-advanced.06_state-management]] | [[\ next \| winc-academy-notes.front-end-course.12_react-advanced.08]] | [[\ Overview \|winc-academy-notes.front-end-course.12_react-advanced.07_talking-to-an-api#overview]] |
| :---------------------------------------------------- | :-----------------------------------------------------------------------------------------: | :--------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------: |
