---
id: xm0ucbdrmhlp6dajgg3ixhf
title: 01. Debugging & Error Boundaries
desc: "course: FE > module: React Advanced > topic: Debugging and Error Boundaries "
updated: 1693384378676
created: 1693383923059
---

<details>

  <summary>Toggle section links</summary>

[[Errors and Warnings|winc-academy-notes.front-end-course.12_react-advanced.01_debugging-and-error-boundaries#errors-and-warnings]]
[[Error Boundaries|winc-academy-notes.front-end-course.12_react-advanced.01_debugging-and-error-boundaries#error-boundaries]]
[[Testing React Components|winc-academy-notes.front-end-course.12_react-advanced.01_debugging-and-error-boundaries#testing-react-components]]

</details>

## Errors and Warnings

**Warnings** do not necessary break the code, but can cause faulty behavior.
**Errors** will cause the code to stop running.

Use **React Dev Tools**: look at the component tree, states, props, list of hooks,
component renders,DOM updates + breakpoints for debugging.

- **Profiler** - inspect how components are being rendered, during app usage/interaction

## Error Boundaries

ErrorBoundary components are similar to chatch blocks in JS
thay catch errors in any of the children components.

- connot be written as a function component => only CLASS component
  (reason: dependence on getDerivedStateFromError and componentDidCatch lifecylcle-methods > no alt in func. comps.)
  - getDerivedStateFromError shows smth. (fallback) instead of the child comp. where the error has happend
  - componentDidCatch is used for logging the error to an error logging service

```javascript
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    //Update state so the next render will show the fallback UI.
    logErrorToMyService(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // We can render any custome fallback UI
      return <h1>The shit has hit the fan.</h1>;
    }

    return this.props.children;
  }
}
```

**Add it anywhere** in the component tree. **Wrap** the component(s) which is/are, or the children of which are expected to fail,
**in** the opening and closing tags of the **ErrorBoundary component** >> the **error will bubble up to the closest error boundary**.
The app will keep running > there will be no need to reload the entire page:

```javascript
const App = () => {
  return (
    <ErrorBoundary>
      <ChildComponent1 />
      <ChildComponent2 />
    </ErrorBoundary>
  );
};
```

## Testing React Components

Use a bundler compatible testing framework.
i.e: Vite > Vitest; Parcel, Webpack > Jest

:warning: the testing framework needs to emulate the browser (React produces a DOM tree)  
For this use: JSDOM (is a Javascript DOM emulator), React Testing Library (**RECOMMENDED**)

Resources:

- [ Jest ](https://jestjs.io/)
- [Vitest](https://vitest.dev/)
- [React Testing Library](https://testing-library.com/docs/react-testing-library/intro)

| [[\ FE notes \| winc-academy-notes.front-end-course]] | [[\ previous \| winc-academy-notes.front-end-course.11_react-basics]] | [[\ next \| winc-academy-notes.front-end-course.12_react-advanced.02_forms]] | [[\ Overview \|winc-academy-notes.front-end-course.12_react-advanced.01_debugging-and-error-boundaries#overview]] |
| :---------------------------------------------------- | :-------------------------------------------------------------------: | :--------------------------------------------------------------------------: | :---------------------------------------------------------------------------------------------------------------: |
