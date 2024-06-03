# Reactive Frontend Workshop

This workshop is made for students of EPITA - SIGL 2025.

The aim of the workshop is to implement **your** reactive user interface (UI) for Sotracteur.

To implement it, we will use:

- [NodeJS](https://nodejs.org/en/about/): to be able to use dependencies (other developper's code)
- [ReactJS](https://fr.reactjs.org/): to manage DOM elements rendering and local component state
- [ViteJS](https://vitejs.dev/): to bundle all our source files into a single `JavaScript` file (that will be our production artifact).

> Note: We are doing only the user interface without any data persistency...for now!

## Step 1: Add `React` and `Vite`

Sofar, we have a responsive application, but we don't handle any user interactions (menu navigation or button clicks).

[ReactJS](https://fr.reactjs.org/) is a JavaScript framework from Facebook and will help us making our application more reactive.

We chose this since it seems to be the most notorious one, when checking [NodeJS framework trends](https://www.npmtrends.com/@angular/core-vs-angular-vs-react-vs-vue-vs-svelte) with its competitors.

### Install the provided template

To avoid tons of copy / paste, we've prepared a small template project to start with `React` and `Vite`.

Follow the README.md instructions on the provided template [EPITA-SIGL2025-SOCRA/reactive-template](https://github.com/EPITA-SIGL2025-SOCRA/reactive-template)

Once done, you should have the following tree of files:

```plain
frontend/
├── .gitignore
├── index.html
├── node_modules
│   ├── ...
├── .nvmrc
├── package.json
├── public
│   └── favicon.ico
├── src
│   ├── App.css
│   ├── App.jsx
│   └── main.jsx
└── vite.config.js

59 directories, 11 files
```

> Note: if you have an issue running `nvm use`, make sure node v21 is installed on your machine.
> To install it `nvm install v21`

### Create your App component

**Objective**: Replace this `Hello SIGL` with **your** application from the the template with your version of Sotracteur from [responsive-frontend workshop](https://github.com/SOCRA-EPITA-SIGL-2024/responsive-frontend).

You already some CSS imported in the `frontend/src/App.jsx` component by the line

```jsx
import "./App.css";
```

For this step, you can put all your CSS code in this `frontend/src/App.css` file.

React `JSX` format is very very close to regular HTML, except attribute are turn in caml case (e.g. `colspan` in html would be `colSpan`), and the `class="mystyle"` attribute is `className="mystyle"` in JSX.

Read more about [how to convert JSX to HTML on react website](https://react.dev/learn/writing-markup-with-jsx#converting-html-to-jsx)

> Note: There is even an online converter to convert your HTML to JSX directly: [html-to-jsx](https://transform.tools/html-to-jsx)

Try to copy paste the content of your `<body></body>` from **your** [responsive-frontend workshop implementation](https://github.com/EPITA-SIGL2025-SOCRA/responsive-frontend) as the return of your `function App() { ... }` in `frontend/src/App.jsx`:

```jsx
import React from "react";
import "./App.css";

function App() {
  return (
    <div className="app">
      <ul className="menu">...</ul>
      <main>
        <form className="search-input">...</form>
        <div className="search-results">...</div>
      </main>
    </div>
  );
}

export default App;
```

> Note: React components, like your `function App`, needs to return only **one** JSX element, with as many children as you like. You can use the React fragment component `<> ... </>`if you do not want to add another div:

```jsx
    function App() {
        return (
            <>
                <h1>Sotracteur<h1>
                <div>...</div>
            </>
        )
    }
```

If you're stuck, feel free to look at the [reactive template (branch step-1_react-vite)](https://github.com/EPITA-SIGL2025-SOCRA/reactive-template/tree/step-1_react-vite).

You should now see same page from **your** [responsive-frontend workshop implementation](https://github.com/SOCRA-EPITA-SIGL-2024/responsive-frontend) but using React, congrats!

## Step 2: Make the menu reactive

You will use [react-router-dom (v6)](https://reactrouter.com/en/6.23.1) JavaScript library to implement it.

- Install `react-router-dom` in your `frontend/` project:

```sh
# from frontend/ directory
nvm use
npm install --save react-router-dom
```

Follow the nice tutorial on [react router documentation](https://reactrouter.com/en/main/start/tutorial) to get familiar with basic concepts.

### Split your components

Reorganize your code to split this big `function App () { ... }` React component into several different React components.

Follow the documentation on [React components](./how-to/REACT-COMPONENTS.md) to know more about React components.

### Implement navigation using react-router

Feel free to look at the [reactive template (branch step-2_react-router)](https://github.com/EPITA-SIGL2025-SOCRA/reactive-template/tree/step-2_react-router) to see how the template uses react router.

- [How to use the `createBrowserRouter` in the template](https://github.com/SOCRA-EPITA-SIGL-2024/frontend-template/blob/ccba9c8e16f5559bd0af32db27d4065e3bce8310/src/Layout.jsx#L8)
  - [How to use the created browser router with the `RouterProvider`](https://github.com/SOCRA-EPITA-SIGL-2024/frontend-template/blob/component-split/src/App.jsx)
- [How to navigate using navigation links](https://github.com/SOCRA-EPITA-SIGL-2024/frontend-template/blob/component-split/src/MainMenu.jsx) with [NavItem custom component (only applying css if the current link is active)](https://github.com/SOCRA-EPITA-SIGL-2024/frontend-template/blob/component-split/src/NavItem.jsx)

## Step 3: Add state management to your app

**Objectives** :

- have a single React component reacting on an action
- have several React components reacting on the same action

### React.useState to manage state for a single React component

Add the following feature:

- when a user clicks on "Réserver" on a tractor in the list, the button text changes state:
  - button become **disabled**
  - ~~Réserver~~ is replaced by **Ajouté au panier**

Create a new `RentButton` React component alongside your component responsible of rendering a Tractor to be rented:

```jsx
function RentButton() {
  const [isRented, setRent] = React.useState(false);
  function onRentClick() {
    setRent(true);
  }

  const buttonText = isRented ? "Ajouté au panier" : "Réserver";

  return (
    <button
      disabled={isRented}
      onClick={() => {
        onRentClick();
      }}
    >
      {buttonText}
    </button>
  );
}
```

Then make to use the `RentButton` React component inside your component responsible of rendering a Tractor to be rented.
For instance, in the reactive template, we are using it inside the `TractorCard` component instead of the former `<button>Réserver</button>`:

```jsx
function TractorCard() {
  return (
    <div className="catalog-item">
      <img src="https://touslestracteurs.com/images/Case_IH/CX90.jpg" />
      <div className="content">
        <span className="category">Une catégorie</span>
        <p>Un exemple de description</p>
        <p>COMMUNE</p>
      </div>
      <div className="actions">
        <RentButton /> {/** this use to be <button>Réserver</button> **/}
        <a>Détail...</a>
      </div>
    </div>
  );
}
```

If you're not familiar at all with `React.useState`, follow the [how to: React.useState ](./how-to/REACT-USE-STATE-HOOK.md) documentation we've prepared for you.

You can have a look at the whole code in the [reactive template (branch step-3a_useState)](https://github.com/EPITA-SIGL2025-SOCRA/reactive-template/tree/step-3a_useState)

### React Context API : have several React components reacting on the same action

Several options are available when it comes to state management: [Redux (for React)](https://react-redux.js.org/), [XState](https://xstate.js.org/docs/recipes/react.html) or the [React Context](https://react.dev/reference/react/useContext) (among others...).

You will use the `React context` to implement state management in your frontend.

Since its implementation is not so trivial, some code is provided to you.

Add a new file `frontend/src/AppContext.jsx` with the following code:

```jsx
import React from "react";

export const initialState = {
  basket: [],
};

export const reducer = (state, action) => {
  switch (action.type) {
    // This `case` is an example of how you can
    // reduce an `action` that adds a new `item` to
    // the `state.basket` array
    case "NEW_BASKET_ITEM":
      const { item } = action;
      return {
        ...state,
        basket: [...state.basket, item],
      };
    default:
      return state;
  }
};

export const AppContext = React.createContext();

function useAppContext() {
  return React.useContext(AppContext);
}

export default useAppContext;
```

And initalize the `AppContext` in your root component (e.g. `App` in the template):

```jsx
import React from "react";
import { AppContext, reducer, initialState } from "./AppContext.jsx";
// ...
// Considering Layout is the name of your root element
// with createBrowserRouter

function Layout() {
  const [state, dispatch] = React.useReducer(reducer, initialState);
  return (
    <AppContext.Provider value={{ state, dispatch }}>
      <div className="app">
        <AppBar />
        <main>
          <Outlet />
        </main>
      </div>
    </AppContext.Provider>
  );
}

export default function App() {
  return <RouterProvider router={router} />;
}
```

Follow the documentation provided [how to use React context](./how-to/REACT-CONTEXT.md) to know how to use the `useAppContext` hooks and help you create a new `Basket` view.

> This documentation explains how to implement the `Add to basket` functionality; which covers partially the objective of the [Challenge 1](#challenge-1-add-to-basket)

## Step 4: Adapt our CD

It's time to deploy all those achievements.

**Objective**: Adapt the CD pipline to deploy the reactive frontend of sotracteur.

### Dockerize the frontend

You need to adapt yout nginx config to [solve the known problem of react-router with Nginx](https://stackoverflow.com/questions/43951720/react-router-and-nginx).

Create a new nginx folder and create a default.conf nginx file inside `frontend/nginx/default.conf` with the following content:

```nginx
server {

    listen       80;
    server_name  localhost;

    location / {
        root   /usr/share/nginx/html;
        try_files  $uri /index.html;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```

- Create a new Dockerfile under `frontend/` directory:

```dockerfile
FROM node:21-alpine as build

WORKDIR /code

COPY package.json .
COPY package-lock.json .

RUN npm ci

COPY . .
RUN npm run build


FROM nginx:1.25.4-alpine

COPY nginx/default.conf /etc/nginx/conf.d/default.conf
COPY --from=build /code/dist /usr/share/nginx/html

EXPOSE 80
```

> Note: this dockerfile uses this nice [multi-stage build feature from Docker](https://docs.docker.com/develop/develop-images/multistage-build/)

- Running `npm run build` will create the `frontend/dist/` folder that will contain all our static JS, HTML, CSS and media files that will be serve by Nginx

- Try out to run the docker container locally:

```sh
# from frontend/
docker build -t sotracteur:react .
docker run -p 8090:80 sotracteur:react
```

Check out your app on [localhost:8090](http://localhost:8090).

### Adapt your github workflow

- Adapt your github workflow to build your docker image from inside `frontend` folder, and you should be set!

Just add `working-directory: frontend` to your build and publish step.

```yaml
build-frontend: # you can rename your build step to be more precise!
  runs-on: ubuntu-latest

  steps:
    - uses: actions/checkout@v3

    - name: build and publish frontend docker image
      working-directory: frontend
      run: # Same as before
```

- Now just commit and push your changes on main, and you should have your new react app deployed, yeay!

## Challenge 1: Add to basket

**Feature**: When a user clicks on the `Réserver` button:

- The `Réserver` button is disabled and text becomes `Ajouté au panier`
- a new `item` is added the the `basket`
- a `Basket` React component displays the new `item` added
- a new `Badge` with the number of item in basket is displayed in the menu (next to the basket menu item)

> Note: except from the last bullet point, there is a lot of help in the [how to: React.useContext](./how-to/REACT-CONTEXT.md) documentation.

## Challenge 2: Remove from basket

**Feature**: When a user selects the `delete` icon of the basket table:

- the item gets removed from the basket table or list (however you've decided to display items in basket)
- the main navigation's item number is decreased by 1 (e.g. from `(2)` to `(1)` or from `(1)` to nothing)
- the tractor becomes available for renting again
