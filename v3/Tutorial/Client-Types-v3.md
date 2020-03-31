- status: complete
- version: 3.x
- follows from: [Game-Sequence](Game-Sequence-v3)

## Overview

Client types are particular implementations of a
[game sequence](Game-Sequence), and each one can be radically
different depending on what its purpose is. Two client types are
mandatory: _player_ and _logic_.

The _player_ type is assigned to participants connecting to the
experiment with a browser. It has the purpose to load the pages of the
game, to handle users' interaction with the screen (e.g. the movement
of the mouse, or the click on a button) and to exchange messages with
the server. 

The _logic_ type is executed on the server and controls the flow of
operations of a game room, such as creating sub-groups, accessing
external datasets, handling player disconnections, etc. Its
implementation obviously varies depending on the experimental design.

Other optional client types can be defined, for example to create
automated players (bots), used to interact with human participants, or
to test the correct functioning of the experiment.

## Implementation

Each client type is defined in a separate file inside the
`game/client_types/` directory. So, for example:

- the _player_ client type is defined in: `game/client_types/player.js`,
- the _logic_ client type is defined in: `game/client_types/logic.js`.

Client types are constructed by adding properties to the "empty"
stages and steps created in the game sequence, and available through
the [stager object](Stager-API-v3).

### Defining a client type

Each client type begins with a function definition:

```javascript
module.exports = function(treatmentName, settings, stager, setup, gameRoom) {
```

where the input parameters mean:

* **treatmentName**: the name of the treatment (chosen by the waiting room)
* **settings**: the game variables associated with the chosen
treatment (as in `game.settings.js`)
* **stager**: a stager object initialized with the sequence from
    `game.stages`,
* **setup**: the nodeGame setup (as in `game.setup.js`)
* **gameRoom**: a reference to the game room object.

The task of the game developer here is to add/extend the properties of
each step (as defined in the game sequence) using the api
`stager.extendStep`.

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
 * **init**, **exit**: see below.

#### Init and Exit Functions

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

### The inheritance chain

An inheritance chain is defined for global, stage and step
properties. That is, steps inherit all the properties defined by the
stage in which they are included; stages inherit all "default"
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

Important! Properties `init` and `exit` are not inherited.

## Next

[Step Callback Functions](Step-Callback-Functions-v3)

## See Also

- The full [Stager API](Stager-API-v3)
