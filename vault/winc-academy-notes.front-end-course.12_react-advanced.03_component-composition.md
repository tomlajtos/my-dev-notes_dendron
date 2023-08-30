---
id: joslzltydprbk6kq7007o0c
title: 03. Component Composition
desc: "course: FE > module: React Advanced > topic: Component Composition"
updated: 1693384634461
created: 1693384259191
---

<details>

  <summary>Toggle section links</summary>

[[Prop Drilling|winc-academy-notes.front-end-course.12_react-advanced.03_component-composition#prop-drilling]]
[[Composing Components|winc-academy-notes.front-end-course.12_react-advanced.03_component-composition#composing-components]]

</details>

## Prop Drilling

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

## Composing Components

### The "children" Prop

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

| [[\ FE notes \| winc-academy-notes.front-end-course]] | [[\ previous \| winc-academy-notes.front-end-course.12_react-advanced.02_forms]] | [[\ next \| winc-academy-notes.front-end-course.12_react-advanced.04_context]] | [[\ Overview \|winc-academy-notes.front-end-course.12_react-advanced.03_component-composition#overview]] |
| :---------------------------------------------------- | :------------------------------------------------------------------------------: | :----------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------: |
