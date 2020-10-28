- status: complete
- version: 5.x
- follows from: [Widgets](Widgets-v5)

## Loading Widgets

Widgets are created and managed with the object `node.widgets`.

```js
// Creating and appending a widget to the page.
var root = W.getElementById('myRoot');
var timer = node.widgets.append('VisualTimer', root);
// Alternative you can directly pass the id of the element.
var timer = node.widgets.append('VisualTimer', 'myRoot');

// Without generating HTML.
var widget = node.widgets.get('MoodGauge');

// Integrating a custom widget.
var options = {};
var customWidget = new MyCustomWidget(options);
node.widgets.append(customWidget, root);
```


### Widget Anatomy

After a widget has been appended, the following HTML div elements are created as
part of the widget interface:

- **panelDiv**: the outer div containing all other elements below.
   - **headingDiv**: the div containing the title of the widget.
   - **bodyDiv**: the div containing the main content of the widget.
   - **footerDiv**: the div containing the footer of the widget (hidden
       by default).

### References to Existing Widgets

After a new widget is created a reference is added inside the array
`node.widgets.instances`.

```js
node.widgets.instances[0]; // VisualTimer.
node.widgets.instances[1]; // MoodGauge.
node.widgets.instances[2]; // CustomWidget.
```

A reference to the last appended widget is stored in under `lastAppended`:

```js
node.widgets.lastAppended; // CustomWidget.
```

### Destroying Widgets

To destroy a widget and removing it from the DOM, just invoke the
`destroy` method on the widget itself.

```js
node.widgets.lastAppended.destroy();

// To destroy all widgets:
node.widgets.destroyAll();
```

#### Garbage Collection

Previously appended widgets that are no longer found on the page are destroyed
upon entering a new step.


## Next

- [See all widgets options](Widgets-Options-v5)
