 - status : complete
 - version : 5.x

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/v5/back-button-widget.jpeg" alt="Illustration of the BackButton widget">
  <br>
</figure>

## Introduction

The BackButton is a customizable button that brings the game one step
back.

The button is automatically disabled for all steps in which the user
is not allowed to go back, such as the initial step, or the first step
of a every new stage if setting `acrossStages` is false.

### Setup

_Important!_ The BackButton should be used only in single-player
stages and when the logic isn't pushing stage updates. That is, you
need to change the default [synchronization rules](Synchronization-v5)
to set the logic as either _observer_ or _follower_.

#### Logic as Observer

The player is free to move back and forth the steps, while the logic
always remains at stage 1.1.1.

This configuration is useful when there are none or just a few
step-specific actions by the logic, which can be defined in the init
function.

Furthermore, this configuration allows a single logic to serve several
independent clients.

##### player.js
```js
// Setting the SOLO rule: game steps each time node.done() is called,
// ignoring the state of other clients.
const ngc = require('nodegame-client');
stager.setDefaultStepRule(ngc.stepRules.SOLO);
```

##### logic.js
```js
// Setting the SOLO rule: game steps each time node.done() is called,
// ignoring the state of other clients.
// The logic will never call node.done() and remain in step 1.1.1.
stager.setDefaultStepRule(ngc.stepRules.SOLO);

// Example of step-specific actions defined in the init function.
stager.setOnInit(function() {
    // Waits for DECISION throughout the whole game.
    node.on.data('DECISION', function(msg) { ... });
});
```


#### Logic as Follower

The player is free to move back and forth the steps, and the logic
always follows suit. This setting is useful when there many
step-specific actions and there is only one client per logic.

##### player.js
```js
// Setting the SOLO rule: game steps each time node.done() is called,
// ignoring the state of other clients.
const ngc = require('nodegame-client');
stager.setDefaultStepRule(ngc.stepRules.SOLO);
```

##### logic.js
```js
// Setting the SOLO rule: game steps each time node.done() is called,
// ignoring the state of other clients.
const ngc = require('nodegame-client');
stager.setDefaultStepRule(ngc.stepRules.SOLO);

// Disabling step syncing for other clients: the logic does not
// push step updates to other clients when it changes step.
stager.setDefaultProperty('syncStepping', false);

// In the init function, we set an event lister to manually follow
// the clients to the same step.
stager.setOnInit(function() {
    // The logic aits for a stage update from the client and then
    // it moves to the same step.
    node.on('in.say.PLAYER_UPDATE', function(msg) {
        if (msg.text === 'stage') {
            setTimeout(function() {
                node.game.gotoStep(msg.data.stage);
            });
        }
    });

    // Last instruction in the init function.
    // Game on clients must be started manually
    // (because syncStepping is disabled).
    setTimeout(function() {
        node.remoteCommand('start', node.game.pl.first().id);
    });
});
```


## Main Options

- **button**: A reference to an existing HTML button, or undefined to
  create a new one.
- **id**: The id of the button, or undefined to equal to 'backbutton'.
- **className**: The name of class for the button, or an array of class
  names, or undefined for the bootstrap classes 'btn btn-lg btn-primary'.
- **text**: The text to display on the button, or undefined for 'I am
  done'.
- **acrossStages**: If TRUE, the back button may go back to a previous
  stage. Default: FALSE.
- **acrossRounds**: If TRUE, the back button may go back to a previous
  round. Default: TRUE.

## Step properties

Whenever a new step is played the `backbutton` property of the new
step is evaluated. If FALSE, the button is disabled. If it is an
object it may contains the following properties:

- **text**: sets the text on the button.
- **enableOnPlaying**: if FALSE a previously disabled button won't be re-enabled
  with a new step. Default: TRUE.

If the property is of type string, it is considered the text of the button.

## Usecase

```js
// Creates a new BackButton widget and and appends it inside the header.
var header = W.getHeader();
var backButton = node.widgets.append('BackButton', header);

```

## Links

- [Source Code](https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/BackButton.js)
- [List of Widgets](Widgets-v5)
