# Error Boundaries

If you do not have any error boundaries within your app, when something breaks the whole UI would break. For this reason React introduced the concept of error boundaries.

Error boundaries catch any errors in their child component tree, log these errors and display a fallback ui. Errors are caught during rendering, in lifecycle methods and in constructors of the whole tree below them.

Error boundaries do **not** catch errors for:

- Event handlers
- Asynchronous code (e.g. setTimeout or requestAnimationFrame callbacks)
- Server side rendering
- Errors thrown in the error boundary itself (rather than its children)

## Error Boundary Components

To make use of error boundaries you would need to setup an class component.It becomes an error boundary if it defines either (or both) of the lifecycle methods `getDerivedStateFromError()` or `componentDidCatch()`.

Use `getDerivedStateFromError()` to render a fallback UI after an error has been thrown. Use `componentDidCatch()` to log error information.

```javascript
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI.
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // You can also log the error to an error reporting service
    logErrorToMyService(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children; 
  }
}
```







