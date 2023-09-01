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
We also have to be careful with component rendering > so wi would not overload the server we're making the request to

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

| [[\ FE notes \| winc-academy-notes.front-end-course]] | [[\ previous \| winc-academy-notes.front-end-course.11_react-basics]] | [[\ next \| winc-academy-notes.front-end-course.12_react-advanced.08]] | [[\ Overview \|winc-academy-notes.front-end-course.12_react-advanced.07_talking-to-an-api#overview]] |
| :---------------------------------------------------- | :-------------------------------------------------------------------: | :--------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------: |
