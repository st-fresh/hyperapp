# Virtual Nodes

A virtual node is a JavaScript object that describes an HTML/DOM tree.

Consider the following HTML.

```html
<main id="app">
  <h1>This is fine.</h1>
</main>
```

And the corresponding virtual node.

```js
{
  tag: "main",
  data: {
    id: "app"
  },
  children: [{
    tag: "h1",
    data: null,
    children: ["This is fine."]
  }]
}
```

To create virtual nodes use the [h](/docs/api.md#h) function.

```js
h("main", { id: "app" }, [
  h("h1", null, "This is fine.")
])
```

Alternatively, use [Hyperx](/docs/hyperx.md) or [JSX](/docs/jsx.md).

The previous example rewritten in Hyperx:

```js
const main = html`
  <main id="app">
    <h1>This is fine.</h1>
  </main>`
```

And in JSX:
```jsx
const main = <main id="app">
  <h1>This is fine.</h1>
</main>
```
