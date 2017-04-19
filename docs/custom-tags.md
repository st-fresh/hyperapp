# Custom Tags

A custom tag is any function that returns a custom virtual node tree.

```js
const Link = (props, children) =>
  h("a", { href: props.href }, children)

h("main", { id: "app" }, [
  h(Link, { href: "#" }, "This is fine.")
])
```

Generates the virtual node below.

```js
{
  tag: "main",
  data: {
    id: "app"
  },
  children: [{
    tag: "a",
    data: {
      href: "#"
    },
    children: ["This is fine."]
  }]
}
```

## JSX

Custom tags and [JSX](/docs/jsx.md) play well together. The example above rewritten in JSX:

```jsx
const Link = (props, children) =>
  <a {...props}>{children}</a>

<main id="app">
  <Link href="#">This is fine.</Link>
</main>
```

