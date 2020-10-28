 - status : complete
 - version : 5.x

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/v5/email-widget.jpeg" alt="Illustration of the EmailForm widget">
</figure>

## Introduction

EmailForm is a widget that displays standard text input for email with
validation.

## Main Options

- **onsubmit**: Options passed to `getValues` when the submit button is pressed.
    Default: `{ feedbackOnly: true, say: true, updateUI: true }`

### Main Texts Options

- **label**: 'Enter your email:'
- **errString**: 'Not a valid email address, please correct it and submit it again.'    

## Main methods

No public methods to call.

## Listeners

None.

## Events

None.


## Usecase

```js
// Creates and append a new DoneButton widget.
var root = document.body;
var emailForm = node.widgets.append('EmailForm', root);

```

## Links

- [Source Code](https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/EmailForm.js)
- [List of Widgets](Widgets-API-v5)
