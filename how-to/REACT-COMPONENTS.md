# React components

This page is to learn more about React components.

## What is a React component?

A React component is a **function** that takes **properties** also called `props` as parameter and return `JSX` elements.

For instance, here is a simple React component that returns a `h1` title; once rendered as HTML:

```jsx
// inside MainTitle.jsx
import React from "react";

function MainTitle(props) {
  return <h1>{props.title}</h1>;
}

export default MainTitle;
```

This component can be written simpler by using [JavaScript destructuring on `props` paramenter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#syntax):

```jsx
// inside MainTitle.jsx
import React from "react";

function MainTitle({ title }) {
  return <h1>{title}</h1>;
}

export default MainTitle;
```

Now you can call this `MainTitle` React components from other React components:

```jsx
import React from "react";
import MainTitle from "./MainTitle.jsx";

function App() {
  return (
    <div>
      <MainTitle title="Sotracteur" />
      <p>Louer des tracteurs pro</p>
    </div>
  );
}

export default App;
```

## One React tree

It means on you **always** have 1 and only 1 main React component.

If you look at the basic `index.html` file, you will have only 1 HTML element (call `root` by convention), on which ReactDOM will bind your React root component (called `App` in our template).

All of your React component will have only one root. If you do not want to create an extra `<div>...</div>` component, but you want to group some childrens together, you can use the [React.Fragment](https://react.dev/reference/react/Fragment) React component; either via the JSX syntax `<>...</>` or the more verbose synthax `<React.Fragment>...</React.Fragment>`.

## How to nest React Components?

To nest several React components together, you can use the special `props` named `children` that is always pass by default to all your React components.

Let's take previous example with `MainTitle` and extend it to a `Header` component that uses nested React components:

```jsx
import React from "react";

// `children` props is passed implicitly to all React components
function MainTitle({ title, children }) {
  return (
    <div>
      <h1>{title}</h1>
      {children}
    </div>
  );
}

function Header() {
  return (
    <MainTitle title="Sotracteur">
      <p>Louer des tracteurs pro</p>
      <p>
        <b>Uniquement quand vous en avez besoin!</b>
      </p>
    </MainTitle>
  );
}

export default Header;
```

Here `children` in the `MainTitle` props are `<p>Louer des tracteurs pro</p>` and `<p><b>Uniquement quand vous en avez besoin!</b></p>` and are rendered by the `MainTitle` React component below the `<h1>{title}</h1>` where the `{children}` placeholder is written.

> Once again, note that the `children` props is implicit and is part of all React components.
