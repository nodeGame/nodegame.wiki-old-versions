- status: complete
- version: 5.x

## Migrating to nodeGame 5.0

### Potentially breaking backward compatibility:

- The [Chat](Chat-Widget-v5) widget has been completely rewritten. Previous
    versions are not compatible.
- `#node.game.getProperty(property, notFound)` has been simplified and does not
  distinguish if a property is non-existing or undefined. Use the `notFound`
  parameter to set the return value for non-existing properties.
- Widgets can no longer specify the `#destroy()` in the prototype;
  `widget.on('destroyed')` should be used instead. 
- Classes specified with the className option in `Widgets.append` or
`Widgets.get` are not added to the default class instead of replacing it.
- Widget `Wall` removed, use `DebugWall`.
- `DoneButton.setText()` removed. Use `DoneButton.button.value`.
- `#node.widgets.destroy()` removed. Use the destroy method on the widget
  itself.

### Non-breaking changes:

- `Window.HTMLRenderer` has been updated to display functions correctly.
- `#node.say()` handles an array for multiple recipients

## Next Topics

Next: [Start the Tutorial v5](Getting-Started-v5)
