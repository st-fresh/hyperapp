# Creating an Application

To create applications use the [app](/docs/api#app) function.

```jsx
app({
  view: () => <h1>Hi.</h1>
})
```

The app function renders the given view and appends it to [document.body](https://developer.mozilla.org/en-US/docs/Web/API/Document/body).

To mount the application on a different element, use the root property.

```jsx
app({
  view: () => <h1>Hi.</h1>,
  root: document.getElementById("app")
})
```

## View and State

A view receives the current state and returns a [virtual node].

```js
app({
  state: "Hi.",
  view: state => <h1>{state}</h1>
})
```

Use the state property to describe your application's data model.

The state can be of any type.

```jsx
app({
  state: ["Hi", "Hola", "こんにちは"],
  view: state =>
    <ul>
      {state.map(greet => <li>{greet}</li>)}
    </ul>
})
```

The view function is called every time the state is modified.

## Actions

Actions are the primary way you can modify the state.

```jsx
app({
  state: "Hi.",
  view: (state, actions) =>
    <h1 onclick={actions.upcase}>{state}</h1>,
  actions: {
    upcase: state => state.toUpperCase()
  }
})
```

To modify the state, an action can return a new state or a part of it.

```jsx
app({
  state: 0,
  view: (state, actions) =>
    <main>
      <h1>{state}</h1>
      <button onclick={actions.addOne}>+1</button>
    </main>,
  actions: {
    addOne: state => state + 1
  }
})
```

You can pass data to actions as well.

```jsx
app({
  state: 0,
  view: (state, actions) => (
    <main>
      <h1>{state}</h1>
      <button
        onclick={() => actions.addSome(1))}>More
      </button>
    </main>
  ),
  actions: {
    addSome: (state, actions, data = 0) => state + data
  }
})
```

Actions are not required to have a return value. You can use them to call other actions, for example after an asynchronous operation has completed.

```jsx
app({
  state: 0,
  view: (state, actions) =>
    <main>
      <h1>{state}</h1>
      <button onclick={actions.addOneDelayed}></button>
    </main>,
  actions: {
    addOne: state => state + 1,
    addOneDelayed: (state, actions) =>
      setTimeout(actions.addOne, 1000)
  }
})
```

An action can return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise). This enables you to use [async functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function).

```jsx
const delay = seconds =>
  new Promise(done => setTimeout(done, seconds * 1000))

app({
  state: 0,
  view: (state, actions) =>
    <main>
      <h1>{state}</h1>
      <button onclick={actions.addOneDelayed}>+1</button>
    </main>,
  actions: {
    addOne: state => state + 1,
    addOneDelayed: async (state, actions) => {
      await delay(1)
      actions.addOne()
    }
  }
})
```

## Events

Events are functions that get called when your application is loaded, before a view is rendered, etc.

```jsx
app({
  state: 2,
  view: state => <h1>{state}</h1>,
  actions: {
    next: state => state * state
  },
  events: {
    loaded: (state, actions) => setInterval(actions.next, 1000)
  }
})
```

You can register multiple event handlers too.

```jsx
app({
  state: 0,
  view: state => <h1>{state}</h1>,
  actions: {
    addOne: state => state + 1
  },
  events: {
    loaded: [
      (state, actions) => actions.addOne(),
      (state, actions) => actions.addOne(),
      (state, actions) => actions.addOne()
    ]
  }
})
```

Events can be useful to hook into the update and render mechanism. For a practical example see the implementation of the [Router].

You can create custom events too.

```jsx
app({
  view: (state, actions) =>
    <button onclick={actions.fail}>Fail</button>,
  actions: {
    fail: (state, actions, data, emit) =>
      emit("error", "Fail")
  },
  events: {
    error: (state, actions, error) => {
      throw error
    }
  }
})
```

The [emit](/docs/api.md#emit) function is the last argument passed to actions and events.

## Mixins

Mixins allow you to reuse functionality between different applications. They can be useful to implement middlewares, plugins, etc.

```jsx
const Logger = () => ({
  events: {
    action: (state, actions, data) => console.log(data),
  }
})

app({
  state: 0,
  view: (state, actions) =>
    <main>
      <h1>{state}</h1>
      <button onclick={actions.addOne}>+1</button>
    </main>,
  actions: {
    addOne: state => state + 1
  },
  mixins: [Logger]
})
```


