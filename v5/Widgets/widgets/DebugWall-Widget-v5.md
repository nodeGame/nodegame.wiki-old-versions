 - status : complete
 - version : 5.x

## Illustration
<figure>
  <img src="http://nodegame.org/images/wiki/v5/debug-wall-widget.jpeg" alt="Illustration of the
  DebugWall widget.">

  <br>
  <figcaption>Appearance of the DebugWall widget in the Monitor interface.</figcaption>
</figure>

## Introduction

Intercepts incoming and outgoing messages and logs, and prints them numbered and
timestamped in div.

Warning! Modifies some core functions, therefore its usage in production is not
recommended.

## Main Options

  - `msgIn`: If FALSE, incoming messages are ignored.
  - `msgOut`: If FALSE, outgoing  messages are ignored.
  - `log`: If FALSE, log  messages are ignored.
  - `hiddenTypes`: An object containing what is currently hidden
     in the wall as keys.
  

## Usecase

```js
// Creates and appends a new DebugWall widget.
var root = document.body;
var chat = node.widgets.append('DebugWall', root);

```

## Links

- [Source Code](https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/DebugWall.js)
- [List of Widgets](Widgets-v5)
