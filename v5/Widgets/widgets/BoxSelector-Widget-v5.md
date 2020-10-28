 - status : complete
 - version : 5.x

## Illustration
<figure>
  <img src="http://nodegame.org/images/wiki/v5/box-selector-widget-closed.jpeg" alt="Illustration of the
  BoxSelector widget closed.">
  <img src="http://nodegame.org/images/wiki/v5/box-selector-widget-opened.jpeg" alt="Illustration of the
  BoxSelector widget once clicked, displaying contained items.">
  <br>
  <figcaption>Appearance of the BoxSelector widget.</figcaption>
</figure>

## Introduction

Displays a configurable button that if clicked opens a menu with a
list of items to select from. The items menu is dynamic, items
can be added and removed any time.

## Main Options

  - `onclick`: A callback triggered when a menu item is clicked.
  - `getDescr`: A callback that returns the item description to be
      displayed in the menu.
  - `getId`: A callback that returns a unique id for each item.

## Main Method

 - `addItem(item)`: Adds an item to the list and renders it into the DOM.
 - `removeItem(id)`: Removes an item with given id from the list and the DOM;
   returns the removed item or FALSE, if not found.

## Usecase

```js
// Create and append a new BoxSelector widget.

var root = document.body;

var bs = node.widgets.append('BoxSelector', root, {
    getId: function(item) {
        return item.id;
    },
    getDescr: function(item) {
        return item.text;
    },
    onclick: function(item, id) {

        // In this example, when items are clicked
        // they get removed, and when all items have
        // been clicked, the box selector is destroyed.

        this.removeItem(id);
        if (this.items.length === 0) {
            this.destroy();
        }
    }
});


// Add items to box selector.
bs.addItem({ id: 1, text: 'Option 1' });
bs.addItem({ id: 2, text: 'Option 2' });
bs.addItem({ id: 3, text: 'Option 3' });
```

## Links

- [Source Code](https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/BoxSelector.js)
- [List of Widgets](Widgets-v5)
