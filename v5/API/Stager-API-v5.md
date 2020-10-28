# Stager API

- status: complete
- version: 5.x

## Overview

The stager defines the game sequence using blocks, stages, and steps.

### Quick Start

Assuming you have access to a Stager object named `stager`, you can
build the game sequence by adding stages, steps and blocks to the
stager sequentially. Important: the order in which you add them
matters! For example:

```javascript
stager.stage('stage 1');

stager.step('step 1.1');
stager.step('step 1.2');
```

create a sequence of one stage with id `stage 1` that contains two steps
with id `step 1.1` and `step 1.2`.

When the game will run `step 1.1` will be execute first, and `step
1.2` second. In this example `stage 1` is just a container for the two
steps.

#### Repeating a stage

It is also possible to repeat a stage a certain number of times. For
example:

```javascript
stager.repeatStage('stage 1', 3);

stager.step('step 1.1');
stager.step('step 1.2');
```

will repeat `stage 1` 3 times, leading to the following game sequence:
`step 1.1`, `step 1.2`, `step 1.1`, `step 1.2`, `step 1.1`, `step
1.2`.


To add more stages just use the `stage` or `repeatStage` multiple
times. For example:

```javascript
stager.stage('stage 1')

stager.repeatStage('stage 2', 3);

stager.step('step 2.1');
stager.step('step 2.2');

stager.stage('stage 3');
```

leads to the following game sequence: `stage 1`, `step 2.1`, `step
2.2`, `step 2.1`, `step 2.2`, `step 2.1`, `step 2.2`, and `stage 3`.

Notice that no step was explicitely added to `stage 1` and `stage 3`. In this
case, a default step named with the same name of the containing stage is added
automatically.


#### Specifying a stage object directly

It is possible to specify the stage and its steps in one command. For example:

```javascript
stager.stage({
    id: 'mystage',
    steps: [ 'step1', 'step2', 'step3' ]
});
```

adds a new stage with id `mystage` containing steps `step1`, `step2`,
`step3` into the sequence. The stage id must be unique. Any step that
is not found in the stager is automatically create with a default
callback.


#### Looping a stage

When you want to repeat stage, but you do not know in advance how many times,
you can specify a loop or a doLoop:

```javascript

// Evaluates condition **before** entering the stage,
// the whole stage can be skipped if the condition is already false.
stager.loopStage('stage 2', function() {
   // Returns true for executing one more iteration of the loop.
   return !this.LOOP_ENDED;
});


// doLoop executes the stage at least once, then it behaves like loop.
stager.doLoopStage('stage 2', function() {
   // Returns true for executing one more iteration of the loop.
   return !this.LOOP_ENDED;
});

```

The loop callback is executed with `node.game` as context. In the above example
the variale `LOOP_ENDED` must be created and set it to true in the logic file to
end the loop. For example:

```javascript

stager.extendStep('stage 2', {
    cb: function() {
        // Example of pseudo-code condition.
        if (TOTAL_PLAYER_PAYOFF > 100) this.LOOP_ENDED = true;
    }
});
```

#### Game Over

Optionally, a game over command can be set at the end of the sequence. For
example:


```javascript
stager.stage('stage 1')

stager.repeatStage('stage 2', 3);

stager.step('step 2.1');
stager.step('step 2.2');

stager.stage('stage 3');

stager.gameover();

```

After the last stage is done, the gameover call will set the stage of the game
to GAME_OVER, and it won't be possible to step back or forth into the game,
unless re-initialized.

The call to game over triggers the `onGameOver` callback, if one has been
defined by the user (see Section "Init and GameOver").


```javascript
stager.onGameOver(function() {
   // Possible actions:
   // - Store results
   // - Move clients to another room
   // - Destroy room
});


```

#### Stage Aliases

The same stage can be played again later in the sequence if you define an
"alias." In the following snippet, we are going to play stage "game" twice,
using the 'AS' keyword to define the "game2" alias.


```javascript
stager.next('game');
stager.next('something_in_betwen');
// Now add an alias.
stager.next('game AS game2');
```


Important! All steps of the aliased stage need to be added _before_ the alias,
see this [issue](https://github.com/nodeGame/nodegame/issues/58).


#### Command Aliases

The following commands are aliases, and can be used interchangeably:

- `.stage`       -> `.next`
- `.repeatStage` -> `.repeat`
- `.loopStage`   -> `.loop`
- `.doLoopStage` -> `.doLoop`


#### Chaining

The stager commands return the stager itself, so it is possible to chain methods
together for a more compact script. The following snippets are equivalent:

```javascript
stager.next('stage1').next('stage2').repeat('stage3', 2);

// is equivalent to:

stager.next('stage1');
stager.next('stage2');
stager.repeat('stage3', 2);

// is equivalent to:

stager.stage('stage1').stage('stage2').repeatStage('stage3', 2)

// is equivalent to:

stager.stage('stage1');
stager.stage('stage2');
stager.repeatStage('stage3', 2);
```

### Step Properties

When a step is executed it exposes a set of properties to its callback function
and to other nodeGame's components.

All step properties are optional, but one: the step callback function (`cb`).


#### Step callback functions

Each step must be associated to a callback function. This can be specified in
three ways: _before_, _after_, or at the time of _insertion_ into the games
sequence, as shown in the examples below.

* Extending the step _after_ it was inserted:


```javascript
stager.extendStep('stage 1', {
   cb: function() { console.log('Hello! I am the step contained in stage 1!'); }
});

stager.extendStep('step 2.1', {
   cb: function() { console.log('Hi! I am step 2.1'); }
});
```

* Specifying the step object _at the time_ of insertion into the sequence:

```javascript
stager.stage({
    id: 'stage 2',
    cb: function() { console.log('Hi! I am the first step of stage 2'); }
});

stager.step({
    id: 'step 2.2',
    cb: function() { console.log('Hi! I am step 2.2'); }
});
```

* Specifying the step object _before_ inserting it into the sequence:

```javascript
stager.createStage({
    id: 'stage 2',
    cb: function() { console.log('Hi! I am the first step of stage 2!'); }
});

stager.createStep({
    id: 'step 2.2',
    cb: function() { console.log('Hi! I am step 2.2'); }
});

// Must follow something like:
// stager.stage('stage 2').step('step 2.2');
```

#### Other step properties

Here is the list of standard nodeGame properties and their explanation:

* `timer`: number|object <br>
   Configures the Visual Timer object, if any. If the parameter is a
   number, it specifies the duration of the timer in milliseconds. If
   object, it allows full configuration of the timer. For details, see
   [Visual Timer](VisualTimer-Widget).

* `done`: function <br>
   Performs a final checking before stepping. The function will be
   executed when `node.done()` is called, and if it returns `TRUE`,
   the step-rule will be invoked, otherwise the stepping process is halted.

* `init`: function <br>
   Defines an initialization function executed before each stage /
   step. Listeners registered here are available throughout the stage
   / step.

* `exit`: function <br>
   Defines an exit function executed before after the stage / step
   was completed, and before the next one begins. Listeners registered
   here are deleted immediately.

* `minPlayers`: array [number, function] <br>
   Defines the minimum number of players that need to be connected
   simultaneously in order to continue the game. As soon as the
   requirement is not met the callback function is invoked.

* `maxPlayes`: array [number, function] <br>
   Defines the maximum number of players that need to be connected
   simultaneously in order to continue the game. As soon as the
   requirement is not met the callback function is invoked.

* `exactPlayers`: array [number, function] <br>
   Defines an exact number of players that need to be connected
   simultaneously in order to continue the game. As soon as the
   requirement is not met the callback function is invoked.

* `globals`: object <br>
   Defines properties that will be available at run-time under
   `node.game.globals`.
   
* `frame`: string|object <br> An HTML resource to load in the
   frame. Available only if nodeGame client is executed in the
   browser. See the `loadFrame` method of the
   [Window API](Window-API-v5).

* `stepRule`: function|string <br>
   Defines the condition for the client to enter into the next step / stage.
   If the function returns `TRUE`, the next stage in the game-sequence will
   be executed.

#### Step properties inheritance

If a step property is defined at the level of the stage, it is
automatically inherited by all steps contained within the stage

If a property is defined at the level of the game, it will be valid
for all stages and steps (unless overwritten locally).

For example:

```javascript

// Methods that define step properties valid throughout a whole game:

// Sets the default step-rule function that never steps forward (wait rule).
stager.setDefaultStepRule(function() { return false; } );

// Sets the default globals.
stager.setDefaultGlobals({
   a: 1,
   b: 2
});

// Sets a default property (see also stager.setDefaultProperties).
stager.setDefaultProperty('timer', 30000);

// Stage defining a minPlayer property valid for all nested steps.
stager.createStage({
    id: 'stage1',
    cb: [ 'step 1.1', 'step 1.2', 'step 1.3' ],
    minPlayers: [ 4, function() { console.log('Not enough players!') } ]
});

// Step overwriting the timer and minPlayers properties with own values.
stager.step({
    id: 'step 1.3',
    cb: function() { console.log('I am step 1.3'); },
    minPlayers: undefined,
    timer: 3000
});
```

defines a stage with 3 nested steps. 


### Init and Gameover

It is possible to specify an init and clean-up function executed when
the stager is entering the `init` and `gameover` state of the
sequence.

* `setOnInit(func)`: Sets the init function. The function will be
called as soon as the game is instantiated, i.e. at stage 0.0.0. Event
listeners defined here stay valid throughout the whole game, unlike
event listeners defined inside a stage / step, which are valid only
within the specific stage / step.

* `setOnGameover(func)`: Sets the gameover function. This function is
called after the last stage of the game-sequence is terminated.


#### Updating stages and steps

All already defined stages and steps can be easily updated with the
methods: `extendStage`, `extendStep`. They accept either an update
object or an update function. The update object simply add new
properties to existing stage/step object. If a property with the same
name exists it will silently overwritten. The update function allows
to create more complex inheritance patterns, retaining the existing
properties of the extended object. For example.


```javascript
// Update stage

// Create a stage with one default step called 'default stage'.
stager.stage('default stage');

// Extend step with an update object.
stager.extendStep('default stage', {
    cb: function() {
        console.log('I have an interesting callback now!');
    },
    timer: 2000
});

// Extend step with an update function.
stager.extendStep('default stage', function(o) {
    o._cb = o.cb;
    o.cb: function() {
        // Call existing parent callback function.
        this.getCurrentStepObj()._cb();
        // Do something else.
        console.log('I am even more interesting!');
    },
    o.timer: 10000;
    return o;
});
```

If an attempt to update the id of the stage/step is made an error will
be thrown.


## Advanced Commands



### Blocks and Positions

It is possible to have more control on the game sequence specifying
 the _positions_ that stages and steps are allowed to take. When a
 position is specified the order in which stages and steps are
 inserted might lose importance For example:

```javascript

stager.stage('stage 1');

// * means take any available position.
stager.step('step 1.1', '*');
stager.step('step 1.2', '1');
stager.step('step 1.3', '*');
```

defines a sequence of one stage with three steps, one of which is
fixed in position 1, and the other two can assume a variable
position. 

When the game runs it will follow a variable game sequence which will
alternate between two possibilities: `step 1.1`, `step 1.2`, `step
1.3`, and `step 1.3`, `step 1.2`, `step 1.1`.

Positions can be specified for stages as well, and the position
parameter is always the last one. Position can be specified as an
expression which will be evaluated.

```javascript
// Place stage 1 after position 0.
stager.stage('stage 1', '>0');

stager.step('step 1.1', '*');
stager.step('step 1.2', '1');
stager.step('step 1.3', '*');

// When no position is specified, the order
// of insertion into the sequence counts.
stager.stage('stage 2');

stager.repeatStage('stage 3', 2, '*');
```

leads to the sequence: `stage 3`, `stage 2`, `step 1.1`, `step 1.2`,
`step 1.3`, or `stage 3`, `stage 2`, `step 1.1`, `step 1.2`, `step
1.3`, `step 1.3`, `step 1.2`, `step 1.1` (note the steps 1.1 and 1.3
are switched).


#### Blocks

_Blocks_ group together stages or steps and define the scope within
which the position parameter applies. For example:

```javascript

// Place this block of stages at position > 0.
stager.stageBlock('>0');

stager.stage('stage 1');

// This step can at any position within stage 1.
stager.step('step 1.1', '*');

// This block of steps must be at position 0 within stage 1.
stager.stepBlock('0');
stager.step('step 1.2');
stager.step('step 1.3');

// This block of steps must be at position 1.
// Notice that blocks can optionally be given an id.
stager.stepBlock('Step Block 2', '1');
stager.step('step 1.4');
stager.step('step 1.5');

// Place this block of stages anywhere in the sequence.
stager.stageBlock('*');

// Place at position 0 or 1.
stager.stage('stage 2', '0..1');

stager.stage('stage 3', '2');
stager.step('step 3.1', '*');
stager.step('step 3.2', '*');

stager.stage('stage 4', '*');                        
```

would create the sequence (among others): `stage 4`, `stage 2`, `step
  3.2`, `step 3.1`, `step 1.2`, `step 1.3`, `step 1.4`, `step 1.5`,
  `step 1.1`.


### Setting the Game Plot on Clients

The stager's state can be directly used to setup local or remote
clients. When the game-sequence is passed onto clients is named _Game
Plot_.

```javascript
var state2 = stager2.getState();

// Setups locally the game-plot.
node.setup('plot', state2);

// Setups remotely the game-plot on client 'client'.
node.remoteSetup('plot', 'client', state2);
```

The game-plot will be used at run-time to execute the game as defined
by the stager.

The setup of the game-plot can be done automatically when a client
enters a new game room if the plot is exported by one of the
game-paths defined in the `game.settings.js` file. For details, refer
to the [User Guide](User-Guide-v09#settings).


### Create a Stager Object

Load the nodegame-client library and request a new stager instance.

```javascript
var ngc = require('nodegame-client');
var stager = ngc.getStager();
```

### Stager State

After all the stages have been added, and the sequence created, it is
possible to fetch the stager state with `stager.getState()`. The state
can be passed directly to the constructor of a new `Stager` object, to
create a copy that can be extended.

```javascript
var state = stager.getState();

// Variable state contains the stages, the steps,
// and the sequence, as previously defined.

var stager2 = ngc.getStager(state);

// Extend stager2 as needed.
```

### Flexible Execution Mode (Experimental)

In the *standard* execution mode, the sequence of all the stages of a
game must be completely defined before the game starts.

However, it is also possible to decide the next stage of the game
dynamically at run-time. This way the actual sequence played can
depend on players's actions. This is called the *flexible* execution
mode.

* `registerGeneralNext(func)`: Sets general callback for next stage decision.
   Available only when nodegame is executed in *flexible* mode.
   The callback given here is used to determine the next stage.
* `registerNext(id, func)`: Registers a step-decider callback for a specific
   stage. The function overrides the general callback for the specific stage,
   and determines the next stage. Available only when nodegame is executed in
   *flexible* mode.

The `next` functions must return the id of the next stage to be
executed or `'NODEGAME_GAMEOVER'` in case the game is over.
