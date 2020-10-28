- status: complete
- version: 5.x

## Overview

Synchronization is achieved by the division of the game sequence into
stages and steps, and by deciding which _step-rule_ applying for each
of them.  Different step-rules can be applied at each step, giving a
powerful tool to achieve synchronization among connected clients.

### Step-rules

A step-rule is a function specifying the condition for entering into
the next game stage or step. For example, a step-rule might make a
player wait for all others to finish the same step, and another could
make it step forward freely until the next synchronization checkpoint.

Step-rules are evaluated whenever a certain type of event is emitted,
for example: `node.done()` is called, or when the client receives a
state update by another player, or if explicitly requested to do so.

#### Applying step rules

Step-rules are applied directly to steps and stages in the definition
of the game sequence. For example:

```javascript

var ngc = require('nodegame-client');
var stepRules = ngc.stepRules;

// stager definition ...

stager.extendStep('mystep', {
    stepRule: stepRules.WAIT
});
```

will make a client wait for an explicit command to step from the
server. This is the default step-rule for players in all steps, and
does not need to be specified.

If you would like the players to step automatically when `node.done()`
is called you should specify the _SOLO_ step-rule.

```javascript

// stager definition ...

stager.extendStep('mystep', {
    stepRule: stepRules.SOLO
});
```

The default step-rule of the logic is _SYNC_STEP_, that is waiting for
all the connected players to have finished the current step; when this
happens the logic steps and sends the message to synchronously step to
all connected players.

#### Default Step Rules

* `WAIT` waits for a message to step.
* `SOLO` advances freely through the steps, regardless of other
  connected players.
* `SYNC_STEP` waits for all the players to have terminated the
current step.
* `SYNC_STAGE` advances freely trough the steps of the same stage,
but waits before entering a new stage.

#### Custom step-rules

A step-rule function must return TRUE to make the client enter the
step, or FALSE to make the client wait. It accept four input
parameters that could be useful for checking conditions:

* step _{object}_ The current game step
* stageLevel _{number}_ The current stage level (100 = DONE)
* pl _{PlayerList}_ A reference to the list of connected players
* game _{Game}_ A reference to the whole game object

For example:

```javascript
function myStepRule(step, myStageLevel, pl, game) {
   if (myStageLevel !== node.constants.stageLevels.DONE) return false;
   if (!pl.isStepDone(step)) return false;
   return true;
}
```

returns TRUE only if the client itself and all other connected players
are `DONE`.

### Example: Single/concurrent game play

To create a game in which players play independently from each other (for instance a labeling task), simply add the line below to both `player.js` and `logic.js`:

```js
// Set the default step rule for all the stages.
// Assuming you have at the top: const ngc = require('nodegame-client');
stager.setDefaultStepRule(ngc.stepRules.SOLO);
```

The players will now step freely through the game steps, while the logic remain on the first stage (1.1-1).

These settings may require to adjust your code. In the logic, you should insert all your event listeners in the init function of the game, while in the player you may use `node.say` to signal specific game events (for instance, game ends).

The logic can listen to individual players changing steps with the following listener:

```js
node.on.data('done', function(msg) {
    console.log('Client ' + msg.from + ' finished stage ' + msg.stage);
});
```

In addition, you should disable the wait screen on players (because they are not waiting for other players):
```js
W.init({ waitScreen: false });
```

Sometimes in single-player games, you may want to create just one logic for all clients. If so, add this option to the `waitroom.setings.js` file:

```js
DISPATCH_TO_SAME_ROOM: true
```


## Sources

- [stepRules.js](https://github.com/nodeGame/nodegame-client/blob/master/lib/modules/stepRules.js)

## Go Back to

* [Home](Home)
