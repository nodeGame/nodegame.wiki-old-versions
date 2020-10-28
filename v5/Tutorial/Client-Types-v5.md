- status: complete
- version: 5.x
- follows from: [Game-Sequence](Game-Sequence-v5)

## Overview

Client types implement the [game sequence](Game-Sequence). Each client type can be radically different depending on what its purpose is (player, bot, phantom, or logic).

Two client types are mandatory: _player_ and _logic_.

- _player type_: is assigned to participants connecting to the
experiment with a browser. It has the purpose to load the pages of the
game, to handle users' interaction with the screen (e.g., the movement
of the mouse, or the click on a button) and to exchange messages with
the server.

- _logic type_: is executed on the server and controls the flow of
operations of a game room, such as creating sub-groups, accessing
external datasets, handling player disconnections, etc.

Other optional client types can be defined, for example to create
automated players (bots), used to interact with human participants, or
to test the correct functioning of the experiment.

## Implementation

Each client type is defined in a separate file inside the
`game/client_types/` directory. So, for example:

- the _player_ client type is defined in: `game/client_types/player.js`,
- the _logic_ client type is defined in: `game/client_types/logic.js`.

Client types are constructed by adding step properties to the "empty"
stages and steps created in the game sequence and available through
the [stager](Stager-API-v5).

### Defining a client type

Each client type is wrapped in a function definition:

```javascript
module.exports = function(treatmentName, settings, stager, setup, gameRoom) {

    // Client type code here.
};
```

where the input parameters mean:

* **treatmentName**: the name of the treatment (chosen by the waiting room)
* **settings**: the game variables associated with the chosen
treatment (as in `game.settings.js`)
* **stager**: a stager object initialized with the sequence from
    `game.stages`,
* **setup**: the nodeGame setup (as in `game.setup.js`)
* **gameRoom**: a reference to the game room object.



### Adding step properties

The task of the game developer is to add _step properties_ for
all the steps defined in the game sequence.

Step properties are variables that are read by the nodeGame engine when the game reaches a given step. They are divided in two groups:

- _default_: the nodeGame engine reads them and automatically executes some routines; for instance, `frame` property contains the name of the html page to load (will learn more about these later).
- _user-defined_: the specific design of the game requires them; more info below.

For this purpose, the following APIs are available:

- `stager.extendStep`: adds a property to a step,
- `stager.extendStage`: adds a property to a stage,
- `stager.setDefaultProperty`: adds a property for the whole game,
- `stager.setOnInit`: adds a function executed before the game starts
- `stager.setOnGameover`: adds a function executed after the game ends.


For instance:

```javascript
stager.extendStep('instructions', {
    coins: settings.COINS
});
```
adds a game variable into the "instructions" step. Notice that variable is coming from the `settings` object, which is received in input by the client type function.


### The inheritance chain

All _user-defined_ and most of the _default_ step properties are inherited following the "inheritance chain" game-stage-step. That is, steps inherit all the properties defined by the stage in which they are included; stages inherit all "default"
properties defined at the game level.

For example, if we define the property "coins" as follows:


```javascript
stager.setDefaultProperty("coins", 100);
stager.extendStage("game", { coins: 200 });
stager.extendStep("decision", { coins: 50 });
```

Then, during the game, invoking `node.game.getProperty("coins")` would
return 50 in the step "decision" and 200 in the step "results" of the "game"
stage, and 100 in any other step.


### Event listeners validity

Event listeners are functions that are executed when a given event takes place (for instance a mouse click).

nodeGame event listeners are registered with the `node.on` api, you can check some examples [here](Data-Exchange-Examples).

Event listeners follow the same inheritance rule of step properties. That is, event listeners registered inside the init (or callback) function of a
stage / step are valid only for the duration of the stage / step. To
register an event listener valid throughout the game, set an
initialization function of the game.


```javascript
stager.setOnInit(function() {
    // When the event 'myEvent' is fired, this code is executed.
    node.on('myEvent', function() {
        console.log('I am valid throughout the whole game.');
    });
});
```


## Next

[Default Step Properties](Default-Step-Properties-v5)

## See Also

- The full [Stager API](Stager-API-v5)
