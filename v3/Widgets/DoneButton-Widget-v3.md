 - status : complete
 - version : 3.x

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/done-button-widget.jpeg" alt="Illustration of the DoneButton widget">
  <br>
  <figcaption>The DoneButton is a customizable button to call for the end
  of a step.</figcaption>
</figure>

## Introduction

The DoneButton is a customizable button to call for the end of a step.

## Main Options

- **button**: A reference to an existing HTML button, or undefined to
  create a new one.
- **id**: The id of the button, or undefined to equal to 'donebutton'.
- **className**: The name of class for the button, or an array of class
  names, or undefined for the bootstrap classes 'btn btn-lg btn-primary'.
- **text**: The text to display on the button, or undefined for 'I am
  done'.
  
## Main methods

**setText**: Sets the text of the button.

## Step properties

Whenever a new step is played the `donebutton` property of the new
step is evaluated. If FALSE, the button is disabled. If it is an
object it may contains the following properties:

- **text**: sets the text on the button.
- **enableOnPlaying**: forces disabling the button.

## Usecase

```js
// Creates and append a new DoneButton widget.
var root = document.body;
var doneButton = node.widgets.append('DoneButton', root);

```

## Links

- [List of Widgets](Widgets-v3)
