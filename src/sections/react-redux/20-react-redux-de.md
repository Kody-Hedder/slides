# React mit Redux

## React mit Redux

https://redux.js.org/basics/usage-with-react

Setup: `npm install redux react-redux`

Typescript: `npm install @types/react-redux`

## Presentational und Container Components

- presentational components: "Normale" React-Komponenten (wiederverwendbar)
- container components: Zugriff auf Redux-Store / Mit dem Redux-Store verbunden

## React-Redux: < Provider >

Provider: Hinzufügen von Redux-Store zu einer React-App

## React-Redux: < Provider >

```js
// index.js
import { Provider } from 'react-redux';

[...]

ReactDOM.render(
  <Provider store={myStore}>
    <App/>
  </Provider>
  ...
);
```

## Counter: Connect

connect: verbindet React-Komponenten mit dem Redux store

- `mapStateToProps`: verbindet React props mit Redux state
- `mapDispatchToProps`: verbindet React props mit Redux actions

Aufruf:

```js
const ConnectedComponent = connect(
  mapStateToProps,
  mapDispatchToProps
)(Component);
```

## Counter: Connect (state)

```jsx
import { connect } from 'react-redux';

const mapStateToProps = (state) => ({ count: state });
}

[...]
    <div className="App">
      {JSON.stringify(this.props)}
    </div>
[...]

export default connect(mapStateToProps)(App);
```

## Counter: Connect (actions)

```jsx
const mapDispatchToProps = dispatch => ({
  // dispatch ist die dispatch-Funktion des Stores.
  // sie wird uns hier mittels dependency injection
  // zur Verfügung gestellt
  increment: () => dispatch({ type: 'INCREMENT' }),
  decrement: () => dispatch({ type: 'DECREMENT' }),
});
```

```xml
<button onClick={this.props.increment}>+</button>
<button onClick={this.props.decrement}>-</button>
```

## Counter: Dispatch mit TypeScript

```ts
import { Action, Dispatch } from 'redux';

interface MyAction extends Action {
  payload: any;
}

const mapDispatchToProps = (
  dispatch: Dispatch<MyAction>
) => ({
  increment: () => {
    dispatch({ type: 'INCREMENT', payload: 1 });
  },
});
```

## Redux mit TypeScript

siehe:

- https://github.com/piotrwitek/react-redux-typescript-guide
- https://medium.com/@resir014/a-type-safe-approach-to-redux-stores-in-typescript-6474e012b81e
- https://www.carlrippon.com/strongly-typed-react-redux-code-with-typescript/