 - status : complete
 - version : 4.x

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/feedback-widget.jpeg" alt="Illustration of the Feedback widget">
</figure>

## Introduction

Feedback is a widget that displays standard textarea form with validation.

## Main Options

- **maxLength**: An integer that denotes the max character length the client
    should submit for feedback. This is used in the feedback input validation,
    to prevent excessive submissions, but should also be checked in the
    logic. Default: `800`
- **minLength**: As maxLength, but minimum length. Default: `1`
- **maxAttemptLength**: The max length for stored attempts to submit a feedback.
    Default: `2000`
- **showCharCount**: If TRUE, the character count is shown. Default: true
- **onsubmit**: Options passed to `getValues` when the submit button is pressed.
    Default: `{ feedbackOnly: true, say: true, updateUI: true }`

### Main Texts Options

- **label**: 'Any feedback about the experiment? Let us know here:',
- **sent**: 'Sent!'

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
var feedback = node.widgets.append('Feedback', root);

```

## Links

- [List of Widgets](Widgets-API-v4)
