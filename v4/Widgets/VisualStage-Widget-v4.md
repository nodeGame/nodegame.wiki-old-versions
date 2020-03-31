 - status : complete
 - version : 4.x

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/visual-stage-widget.jpeg" alt="Illustration of the VisualStage widget">
</figure>

## Introduction

The VisualStage displays the name of previous, current, and next stage.
  
## Main methods

- **writeStage**: updates the display.

## Listeners

- **STEP_CALLBACK_EXECUTED**: updates the display.

## Usecase

```js
// Creates and append a new VisualStage widget.
var root = document.body;
var chat = node.widgets.append('VisualStage', root);

```

## Links

- [List of Widgets](Widgets-v4)
