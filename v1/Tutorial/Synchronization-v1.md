- status: complete
- version: 1.x
- follows from: [Game Advanced](Game-Advanced-v1)

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

  
## Next

* [Learn about how to dynamically create pages and views](Views-v1)


## See Also

- Source code [stepRules.js](https://github.com/nodeGame/nodegame-client/blob/master/lib/modules/stepRules.js)
