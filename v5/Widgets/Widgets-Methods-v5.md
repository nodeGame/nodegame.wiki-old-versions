- status: complete
- version: 5.x
- follows from: [Widgets](Widgets-Options-v5)

## Default methods

All widgets implement a set of standard methods:

### Get and set values

- **getValues**: gets/sets the current selection of the widget (when available).
- **setValues**: gets/sets the current selection of the widget (when available).
- **reset**: resets the internal state of the widget and its appereance.

### Alter appearance

- **highlight**: highlights the widget in some way, usually indicating an error;
  emits `highlighted`.
- **unhighlight**: undoes the call to highlight; emits `unhighlighted`.
- **hide**: makes the widget completely invisible; emits `hidden`.
- **show**: makes the widget visible; emits `shown`.
- **toggle**: shows the widget if hidden, and vice versa.
- **disable**: the widget stays visible, but user cannot interact with it; emits
  `disabled`.
- **enable**: undoes a previous call to disable; emits `enabled`.
- **collapse**: Collapses the widget (hides the body and footer); emits
  `collapsed`.
- **uncollapse**: Collapses the widget (hides the body and footer); emits
  `uncollapsed`.
- **addFrame**: Adds a border and margins around the bodyDiv element.
- **removeFrame**: Removes a border and margins around the bodyDiv element.

### Query state

- **isHighlighted**: Returns TRUE if widget is currently highlighted.
- **isDocked**: Returns TRUE if widget is currently docked.
- **isCollapsed**: Returns TRUE if widget is currently collapsed.
- **isDisabled**: Returns TRUE if widget is currently disabled.
- **isHidden**: Returns TRUE if widget is currently hidden.
- **isAppended**: Returns TRUE if widget has been appended.
  
### Title, footer, context
  
- **setTitle**: writes content to the HTML title of the widget.
- **setFooter**: writes content to the HTML footer of the widget.
- **setContext**: sets the Bootstrap context class, i.e. 'primary', 'info',
  'success', 'warning', or 'danger.'

### Destroy

- **destroy**: removes any listener defined by the widget, removes HTML created
  by the widget from the DOM, and removes its reference from
  `node.widgets.instances` and `node.widgets.lastAppended`; emits `destroyed`.


### Texts and sounds

- **getText**: Returns the requested text.
- **getTexts**: Returns an object with selected texts.
- **getAllTexts**: Returns an object with all current texts.
- **`setText(requestedText, newText)`**: Checks the value of `requestedText` and assigns the value of `newText` to it for display.
- **setTexts**: Assigns multiple texts at the same time.

- **getSound**: Returns the requested sound path.
- **getSounds**: Returns an object with selected sounds (paths).
- **getAllSounds**: Returns an object with all current sounds.
- **setSound**: Checks and assigns the value of a sound to play.
- **setSounds**: Assigns multiple sounds at the same time.

### Emitting and catching events

- **on**: Registers an event listener for the widget.
- **off**: Removes and event listener for the widget.
- **emit**: Emits an event within the widget (only previously defined events can
  be emitted).

## Next

- [Make a widget-step](Widget-Steps-v5)
