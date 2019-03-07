- status: complete
- version: 2.x
- follows from: [Game-Basics](https://github.com/nodeGame/nodegame/wiki/Game-Basics-v2#client_types)

## Overview

Client types are particular implementations of the game sequence. 

For example, when the _instructions_ stage is played the _player_
client type will display the instruction page and wait for the user to
click on the _continue_ button; at the same time during the
_instructions_ stage the _logic_ client type will simply wait for all
the connected players to be done, and then will advance the game to
the next stage.

## Implementation


### Game Sequence

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


### Client Types

Each client type is defined in a separate file inside the
`game/client_types/` directory. Client types are constructed by adding
properties to the "empty" stages and steps created in the game
sequence, and available through the stager object. 

By default, steps inherit all the properties defined by the stage in
which they are included. Moreover, stages inherit all "default"
properties defined at the stager level. For example, if we define the
property coins as follows:


```javascript
stager.setDefaultProperty("coins", 100);
stager.extendStage("game", { coins: 200 });
stager.extendStep("decision", { coins: 50 });
```

then, during the game, invoking `node.game.getProperty("coins")` would
return 50 in step "decision", 200 in steps "results" inside the "game"
stage, and 100 in any other step.

#### Defining a client type

Each client type begins with a function definition:

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

The task of the game developer here is to add/extend the properties of
each step (as defined in the game sequence) using the api
`stager.extendStep`. One of the most important properties to extend is
the `cb` (callback) property, that defines what will happen in the
step after the page has been loaded. The `cb` property can be overwritten
with the following call:

```javascript
stager.extendStep('instructions', {
    cb: function() {
        // The code defining what the stage instructions will do goes here.
    }
});
```
and new properties can be added as well:

```javascript
stager.extendStep('instructions', {
    // Injecting a game variable into the instructions step.
    coins: settings.COINS
});
```


### Step Properties

Game developers can define their as many properties as needed to steps
and stages, but some names are already reserved by the nodeGame engine
for special purposes. The most important reserved properties are:

 * **frame**: the url of the page to load, or an object containing options.
 * **cb**: the callback (cb) function to be executed after the frame
 is loaded, but before the player can interact with it.
 * **done**: the callback function that is executed when `node.done`
 is executed to terminate a step.
 * **timer**: the duration of the timer for current step (in milliseconds).
 * **timeup**: the callback function execute when the timer of step
 expires.
 * **init**, **exit**: see below

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
just prints the name of the step, does nothing.

It is possible to overwrite the default callback with a custom one:

```javascript
stager.setDefaultCallback(function() {
    console.log('I am the new default callback!');
});
```

The default callback is inserted into all steps that did not define
one when the stager gets finalized, i.e. by calling
`stager.getState()`.

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

Next: [Step Callback Functions](Step-Callback-Functions-v2)

## See Also

- The full [Stager API](Stager-API-v2)
