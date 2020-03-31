- status: complete
- version: 4.x

## Migrating to nodeGame 4.0

### Potentially breaking backward compatibility:

- Window object has updated to the latest version of `JSUS`, meaning that some commands to manipulate and create DOM elements have changed. In general, all new elements should be created using the syntax:
```
var mydiv = W.get('div', {
   id: 'myid',
   foo: 'bar'
});
```
Commands such as `@W.getDiv('myid', { foo: 'nar' }` or `#W.addDiv('myid', { foo: 'bar' })` (insted of div could be any other DOM element) are longer valid and will throw an error.

- `#W.getScreenInfo()` has been removed, use `window.screen` instead.

- `#W.getEventButton()` and `#W.addEventButton()` changed input parameters (following changes in W.get).

- Widgets do not need to call the `init` function explicitly (it is called automatically by the Widget API), therefore it should not be included in the constructor.

- Widgets `css/` folder has been moved inside `nodegame-server/public/stylesheets/`.

- `LanguageSelector` widget sets Window URI prefix and notify server of language selections upon selection.

### Non-breaking changes:

- `node.emit('STEPPING', curStep, nextStep)` also pass old and new step as parameters.

- If you had a `VisualRound` widget in the top header, you probably want to
  disable its title, by specifying `title = false` when you append it.

## Next Topics

Next: [Start the Tutorial v.4](Getting-Started-v4)
