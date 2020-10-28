 - status : complete
 - version : 5.x

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/v5/done-button-widget.jpeg" alt="Illustration of the DoneButton widget">
  <br>
  <figcaption>The DoneButton is a customizable button to call for the end
  of a step.</figcaption>
</figure>

## Introduction

The DoneButton is a customizable button to call for the end of a step.

## Main Options

- **id**: The id of the button. Default: 'donebutton'.
- **button**: A reference to an existing HTML button. Default: it
  creates a new one.
- **className**: The name of class for the button, or an array of class
  names. Default: 'btn btn-lg btn-primary'.
- **text**: The text to display on the button. Default: 'Done'.
- **disableOnDisconnect**: Disable the button upon disconnection. Default: TRUE.
- **delayOnPlaying**: The number of milliseconds to wait before enabling the
  button after the `PLAYING` event is fired (e.g., after a new step begins). Set to zero to enable it immediately. Default: 800.

## Step properties

Whene a new step starts, the step-property `donebutton` is evaluated:

- if equal to _FALSE_, the button is disabled;
- if _string_, it is considered the text of the button,
- if _object_, it may contain the following options (see above for explanations):

    - **text**,
    - **enableOnPlaying**,
    - **delayOnPlaying**.


## Usecases

Create and append a new DoneButton widget to the header.
```js
var header = W.getHeader();
var doneButton = node.widgets.append('DoneButton', header, {
    text: 'Continue'
});
```

Change the text of the button at a given step.
```js
stager.extendStep('stepId', {
    donebutton: {
        text: 'I am done'
    },
    // Other step properties follow as needed.
})
```

Disable the button at a given step.
```js
stager.extendStep('anotherStepId', {
    donebutton: false,
    // Other step properties follow as needed.
})
```


## Links

- [Source Code](https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/DoneButton.js)
- [List of Widgets](Widgets-v5)
