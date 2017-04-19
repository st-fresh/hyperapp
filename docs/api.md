# API

## <a name="h"></a> h(<a href="#h_tag">tag</a>, <a href="#h_data">data</a>, <a href="#h_children">children</a>)

Returns a virtual node.

* <a name="h_tag"></a>**tag**: a tag name as a String, e.g. div or as a Function. See [Custom Tags](#custom-tags).
* <a name="h_data"></a>**data**: an Object with attributes, styles, events, [Lifecycle Events](/docs/lifecycle-events.md), etc.
* <a name="h_children"></a>**children**: a String or an Array of virtual nodes.

## <a name="app"></a> app(<a href="app_props">props</a>)

<a name="app_props"></a>

<pre>
app({
  <a href="#app_state">state</a>,
  actions,
  view,
  events,
  plugins,
  root
})
</pre>

* <a name="app_state"></a>**state**: ...
