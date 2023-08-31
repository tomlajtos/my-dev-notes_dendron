---
id: swat9jk7sq5nypy241gk6ve
title: 02. Forms
desc: "course: FE > module: React Advanced > topic: Forms"
updated: 1693384597869
created: 1693384203280
---

## Overview

<details>

  <summary>Toggle section links</summary>

[[HTML Form elements|winc-academy-notes.front-end-course.12_react-advanced.02_forms#html-form-elements]]  
[[Controlled vs. Uncontrolled Components|winc-academy-notes.front-end-course.12_react-advanced.02_forms#controlled-vs-uncontrolled-components]]  
[[Form Libraries|winc-academy-notes.front-end-course.12_react-advanced.02_forms#form-libraries]]

</details>

## HTML Form elements

i.e.: `<input>`,`<textarea>`,`<select>` etc.

- These elements manage their own state in the browser by default
- On form submission: browser makes a request and navigates to a new page by default
  **We can (and usually want) to intercept this in react** for things like: validate, transform the data before submission
  To do this we have to **CONTROL THE FORM COMPONENTS**

## Controlled vs. Uncontrolled Components

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

## Form Libraries

To avoid writing a lot of boilerplate code it is recommended to use libraries to create forms in React:
i.e.:

- [ Formik ](https://formik.org/)
- [ React Hook Form ](https://react-hook-form.com/)
- [ RJSF ](https://github.com/rjsf-team/react-jsonschema-form)

| [[\ FE notes \| winc-academy-notes.front-end-course]] | [[\ previous \| winc-academy-notes.front-end-course.12_react-advanced.01_debugging-and-error-boundaries]] | [[\ next \| winc-academy-notes.front-end-course.12_react-advanced.03_component-composition]] | [[\ Overview \|winc-academy-notes.front-end-course.12_react-advanced.02_forms#overview]] |
| :---------------------------------------------------- | :-------------------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------: |
