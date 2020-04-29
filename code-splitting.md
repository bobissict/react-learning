# Code Splitting

For code splitting we can use React.lazy. The React.lazy function lets you render a dynamic import as a regular component.

🔗 Documentation: https://reactjs.org/docs/code-splitting.html

⚠️
 This will not work for server rendered apps. For server rendered apps use [Loadable Components](https://github.com/gregberge/loadable-components).

## Importing Components

**Standard Component Import:**

```javascript
import OtherComponent from './OtherComponent';
```

**Import with React Lazy:**

```javascript
const OtherComponent = React.lazy(() => import('./OtherComponent'));
```

This will then only render OtherComponent when the component is rendered.

> React.lazy takes a function that must call a dynamic import(). This must return a Promise which resolves to a module with a default export containing a React component.

⚠️
 React.lazy currently only supports default exports, for other export types check their [documentation](https://reactjs.org/docs/code-splitting.html#named-exports)
 
 ## Suspense
 
 The lazy component should be rendered within a Suspense component. This allows you to add some fallback content (like a loading indicator) while the lazy component is loading.
 
 ```javascript
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}
```

The fallback component accepts any types of React elements. You can place the Suspense component anywhere above the lazy component. You can even wrap multiple lazy components in a single Suspense component.

```javascript
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </div>
  );
}
```

## Error Boundaries

If a module fails to load it will trigger an error. You can handle these errors using Error Boundaries. Once you have created your Error Boundary you can use it anywhere above your lazy components to show an error state.

```javascript
import React, { Suspense } from 'react';
import MyErrorBoundary from './MyErrorBoundary';

const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

const MyComponent = () => (
  <div>
    <MyErrorBoundary>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </MyErrorBoundary>
  </div>
);
```

## Route-based code splitting

Routes are a good place to start with code splitting. Here is an example of code splitting using React Router.

```javascript
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
);
```





 
 
 
 
 
