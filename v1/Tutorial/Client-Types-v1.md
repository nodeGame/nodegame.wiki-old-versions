- status: complete
- version: 1.x
- follows from: [Game-Basics](https://github.com/nodeGame/nodegame/wiki/Game-Basics-v1#client_types)

## Overview

Client types are particular implementations of the game sequence. 

For example, when the _instructions_ stage is played the _player_
client type will display the instruction page and wait for the user to
click on the _continue_ button; at the same time during the
_instructions_ stage the _logic_ client type will simply wait for all
the connected players to be done, and then will advance the game to
the next stage.

## Implementation

Let us assume that the file `game.stages.js` contains the following
game sequence definition:

```javascript
stager.stage('instructions');
stager.stage('verification_quiz');
stager.repeatStage('game', 3);
stager.step('decision');
stager.step('results');
```

At this point all the steps of the sequence have been initialized with
a default callback function (property name `cb`), that the client
types will extend / overwrite.


Inside the `client_types/` directory each client type is contained in
an own file and begins with a function definition:


```javascript
module.exports = function(treatmentName, settings, stager, setup, gameRoom) {
```

The client type has access to following parameters:

* the name of the treatment (chosen by the waiting room)
* the game variables associated with the chosen treatment (as in
`game.settings.js`)
* the nodeGame setup (as in `game.setup.js`)
* a stager object initialized with the sequence from `game.stages`,
* and a reference to the game room object.


Then the `cb` (callback) property of the step object with is
overwritten with the following call:

```javascript
stager.extendStep('instructions', {
    cb: function() {
        // The code defining what the stage instructions will do goes here.
    },
    // Injecting a game variable into the instructions step.
    timer: settings.TIMER
});
```

When extending the step, it is possible to add new properties as well.

```javascript
stager.extendStep('instructions', {
    // Injecting a game variable into the instructions step.
    timer: settings.TIMER
});
```

### Init and Exit Functions

It is possible to execute callbacks just before and after a certain
step is executed. Just add the `init` and `exit` properties to the
step object. For example: 

```javascript
stager.extendStep('instructions', {
    init: function() {
        // See the next section of the tutorial
        // to know more about the `node.on` command.
        node.on('myEvent', function() {
             console.log('I am valid only during this step');
        });
        console.log('I am executed before the step callback');
    },
    exit: function() {
        console.log('I come after the step is done, before new ones begins.');
    }
});
```

Likewise, it is possible to add `init` and `exit` callbacks to the
stage object as well.

Important! Event listeners registered inside the init function of a
stage / step are valid only for the duration of the stage / step. To
register an event listener valid throughout the game, set an
initialization function of the game.


```javascript
stager.setOnInit(function() {
    node.on('myEvent', function() {
        console.log('I am valid throughout the whole game.');
    });
});
```


### Default Callback

Notice that if the callback property (`cb`) of a step is not defined
nor extended, the default callback is applied. The default callback
just prints the name of the step, and calls `node.done()`.

It is possible to overwrite the default callback with a custom one:

```javascript
stager.setDefaultCallback(function() {
    console.log('I am the new default callback, and I do not call node.done()');
});
```

Changing the default callback affects all callbacks that were not
explicitly defined before finalizing the state of the stager, e.g. by
calling `stager.getState()`.

### Returning the State of the Stager

When all necessary steps have been extended, the function returns a
game object containing the state of the extended stager, and the setup
object.


```javascript
    game = setup;
    game.plot = stager.getState();
    return game;
```


## Next Topics

Next: [Step Callback Functions](Step-Callback-Functions-v1)

## See Also

- The full [Stager API](Stager-API-v1)
