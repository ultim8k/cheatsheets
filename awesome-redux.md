---
title: Awesome-redux
category: JavaScript libraries
layout: default-ad
---

### [redux-actions](https://www.npmjs.com/package/redux-actions)
Create action creators in flux standard action format.

```js
increment = createAction('INCREMENT', amount => amount)
increment = createAction('INCREMENT')  // same

err = new Error()
increment(42) === { type: 'INCREMENT', payload: 42 }
increment(err) === { type: 'INCREMENT', payload: err, error: true }
```

### [flux-standard-action](https://github.com/acdlite/flux-standard-action)
A standard for flux action objects. An action may have an `error`, `payload` and `meta` and nothing else.

```
{ type: 'ADD_TODO', payload: { text: 'Work it' } }
{ type: 'ADD_TODO', payload: new Error(), error: true }
```

### [redux-multi](https://github.com/ashaffer/redux-multi)
Dispatch multiple actions in one action creator.

```js
store.dispatch([
  { type: 'INCREMENT', payload: 2 },
  { type: 'INCREMENT', payload: 3 }
])
```

### [reduce-reducers](https://www.npmjs.com/package/reduce-reducers)
Combines reducers (like *combineReducers()*), but without namespacing magic.

```js
re = reduceReducers(
  (state, action) => state + action.number,
  (state, action) => state + action.number
)

re(10, { number: 2 })  //=> 14
```

### [redux-logger](https://github.com/evgenyrodionov/redux-logger)
Logs actions to your console.

Async
-----

### [redux-promise](https://github.com/acdlite/redux-promise)
Pass promises to actions. Dispatches a flux-standard-action.

```js
increment = createAction('INCREMENT')  // redux-actions
increment(Promise.resolve(42))
```

### [redux-promises](https://www.npmjs.com/package/redux-promises)
Sorta like that, too. Works by letting you pass *thunks* (functions) to `dispatch()`. Also has 'idle checking'.

```js
fetchData = (url) => (dispatch) => {
  dispatch({ type: 'FETCH_REQUEST' })
  fetch(url)
    .then((data) => dispatch({ type: 'FETCH_DONE', data })
    .catch((error) => dispatch({ type: 'FETCH_ERROR', error })
})

store.dispatch(fetchData('/posts'))

// That's actually shorthand for:
fetchData('/posts')(store.dispatch)
```

### [redux-effects](https://www.npmjs.com/package/redux-effects)
Pass side effects declaratively to keep your actions pure.

```js
{
  type: 'EFFECT_COMPOSE',
  payload: {
    type: 'FETCH'
    payload: {url: '/some/thing', method: 'GET'}
  },
  meta: {
    steps: [ [success, failure] ]
  }
}
```

### [redux-thunk](https://www.npmjs.com/package/redux-thunk)
Pass "thunks" to as actions. Extremely similar to redux-promises, but has support for getState.

```js
fetchData = (url) => (dispatch, getState) => {
  dispatch({ type: 'FETCH_REQUEST' })
  fetch(url)
    .then((data) => dispatch({ type: 'FETCH_DONE', data })
    .catch((error) => dispatch({ type: 'FETCH_ERROR', error })
})

store.dispatch(fetchData('/posts'))

// That's actually shorthand for:
fetchData('/posts')(store.dispatch, store.getState)

// Optional: since fetchData returns a promise, it can be chained
// for server-side rendering
store.dispatch(fetchPosts()).then(() => {
  ReactDOMServer.renderToString(<MyApp store={store} />)
})
```
