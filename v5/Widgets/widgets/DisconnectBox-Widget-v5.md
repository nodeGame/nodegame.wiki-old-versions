 - status : complete
 - version : 5.x

## Illustration
<figure>
  <img src="http://nodegame.org/images/wiki/v5/disconnect-box-widget.jpeg" alt="Illustration of the
  DisconnectBox widget.">

  <br>
  <figcaption>Appearance of the DisconnectBox widget.</figcaption>
</figure>

## Introduction

Offers features to handle disconnections and reconnections.

## Options

- **showDiscBtn**: If TRUE, a button to disconnect is shown. Default: FALSE.

- **showStatus**: If TRUE, the current status (connected/disconnected) is shown. Default: FALSE.

- **connectCb**: A callback executed when a connection is detected.

- **disconnectCb**: A callback executed when a disconnection is detected.


## Texts

- **leave**: The text on the disconnect button.
- **left**: The text on the button after user has clicked it.
- **disconnected**: Message for status "disconnected."
- **connected**: Message for status "connected."


## Usecase

```js
// Creates and appends a new DisconnectBox widget.
var root = document.body;
var disc = node.widgets.append('DisconnectBox', root, {
    showDiscBtn: true,
    disconnectCb: function() {
        alert('Hey you disconnected!');
    }
});
```

## Links

- [Source Code](https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/DisconnectBox.js)
- [List of Widgets](Widgets-v5)
