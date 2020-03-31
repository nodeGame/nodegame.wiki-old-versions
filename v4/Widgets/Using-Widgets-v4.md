- status: complete
- version: 4.x
- follows from: [Widgets](Widgets-v4)

Common operations are listed below.

## Loading Widgets

Widgets are created and managed with the object `node.widgets`.

```js
// Creating and appending a widget to the page.
var root = W.getElementById('myRoot');
var timer = node.widgets.append('VisualTimer', root);

// Without generating HTML.
var widget = node.widgets.get('MoodGauge');

// Integrating a custom widget.
var options = {};
var customWidget = new MyCustomWidget(options);
node.widgets.append(customWidget, root);
```

After a new widget is created a reference to is added inside the array
`node.widgets.instances`.

```js
node.widgets.instances[0]; // VisualTimer.
node.widgets.instances[1]; // MoodGauge.
node.widgets.instances[2]; // CustomWidget.
```

To destroy a widget and removing it from the DOM, just invoke the
`destroy` method.

```js
node.widgets.instances[0].destroy();
```

## Default methods

All widgets implement a set of standard methods:

- **hide/show**: makes the widget visible or invisible.
- **disable/enable**: the widget stays visible, but it is not
  clickable.
- **unhighlight/highlight**: highlights the widget in some way, or
  restores its appearance to default.

- **getValues/setValues**: gets/sets the current selection of the
  widget (when available).
- **destroy**: removes any listener defined by the widget, removes
  HTML created by the widget from the DOM, and removes its reference
  from \texttt{node.widgets.instances}.
- **setTitle**: writes content to the HTML title of the widget.
- **setFooter**: writes content to the HTML footer of the widget.
- **setContext**: sets the Twitter-Bootstrap context, i.e. 'primary',
  'info', 'success', 'warning', or 'danger.'

## Default Properties

After a widget has been appended to the DOM, the following references
to the HTML elements of the interface are added as properties of the
widget:

- **panelDiv**: the main div containing all sub-elements.
- **headingDiv**: the div containing the title of the widget.
- **bodyDiv**: the div containing the main content of the widget.
- **footerDiv**: the div containing the footer of the widget (hidden
  by default).

## Next

- [Make a widget-step](Widget-Steps-v4)
