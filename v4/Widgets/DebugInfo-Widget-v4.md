 - status : complete
 - version : 4.x

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/debug-info-widget.jpeg" alt="Illustration of the DebugInfo widget">
</figure>

## Introduction

The DebugInfo widget displays information about the current state of
the node instance.

## Main Options

- **intervalTime**: refresh time in milliseconds for the
    widget. Default: 1000.
  
## Main methods

- **updateAll**: updates all information.


## Usecase

```js
// Creates and append a new DebugInfo widget.
var root = document.body;
var chat = node.widgets.append('DebugInfo', root);

```

## Links

- [List of Widgets](Widgets-v4)
