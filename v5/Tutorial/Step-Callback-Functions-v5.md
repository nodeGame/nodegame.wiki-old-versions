- status: complete
- version: 5.x
- follows from: [Client Types](Client-Types-v5)

## Overview

The step callback is defined in the `cb` (callback) step property
associated with a step. The step callback is executed after a step has
been initialized, and its frame -- if defined -- fully loaded. The
user cannot yet interact with the page until the execution of the step
callback is finished.

Step callbacks have two purposes:

- handling user interaction with the page,
- terminating the step with `node.done()`

## Implementation

As any other step property, the step callback is defined by extending
a step of the game sequence using the [Stager API](Stager-API-v5).

```javascript
stager.extendStep('instructions', {
    cb: function() {
        console.log('I am the step callback.');
        console.log('I decide when terminate this step!');
        // The user clicks a button when he/she finished
        // reading the instructions page.
        var button = W.getElementById('read');
        button.onclick = function() {
            node.done('ok!');
        };
    }
});
```

### Handling user interaction

In a step callback the game developer usually makes use of the
[Window API](Window-API-v5) to handle the interaction of the user with
the page, and of other methods of the [Client API](Client-API-v5) to
exchange game messages and perform other game-related tasks.

**Note**: Compared to Stager API, which begins with stager, the Window API begins with W. The Stager is used to create the game sequence _before_ the game starts, while the Window is used to manipulate the page when the game is running. Notice that the Stager API accepts callbacks which may include other API, simply because they are not executed at the same time. On the browser, nodeGame runs through the stages defined by the Stager API and executes the functions included therein, which may contain, for instance, W. commands.

The API is available through the following objects:

- `node`: it is the higher-level container of all operations. Contains
  methods to connect/disconnect from a server, send and receive
  messages, and a reference to the prototypes of all nodeGame classes.
- `node.game`: it is the context of execution of the step callback
function (in other words, the `this` reference is a pointer to
`node.game`). Contains the memory database and the player list and
methods to control the flow of the game.
- `node.window`: it is available when the step callback is executed in
 the browser, (and its shortcut `W`), exposing methods to manipulate
 the browser's window.
- `node.widgets`: contains methods to load and manage widgets (some
  widgets are available only in the browser).
- `node.timer`: contains methods to set timers and emit random events.

#### Summary of the most common API operations

* sending game messages: 

```javascript

    // node.say

    // Sends a message labeled 'Hello!' to the game room's logic.
    node.say('Hello!');
    // Send a message labeled 'offer' to a certain receiver,
    // with a data value equal to 100.
    node.say('offer', receiverId, 100);
    
```


* setting a timer:

```javascript
   // Creates a timer that will emit after 10 seconds
   // the local event 'TIMEUP'.
   var timer = node.timer.createTimer(10000);
   timer.start();
```

* registering event listeners:

```javascript
   // node.on.data
   
   // Listens to incoming message of type 'data'
   // For example, node.say sends messages of type 'data'.

   node.on.data('Hello!', function(msg) {
       console.log('Somebody says hello: ', msg.from);
   });   

   // node.on
   
   // Listens to local events
   
   node.on('TIMEUP', function() {
       console.log('The timer expired.');
   }); 
```

Important! An event listener registered inside a step callback
function will always catch all the events generated by incoming
messages in the same step. In fact, messages arriving before the
execution of the step callback function is terminated (including
asynchronous loading of a frame) are buffered and emitted later.

* accessing the game dataset (usually on the logic client type):

```javascript
   // Select all offers greater than 10.
   var db = node.game.memory.select('offer', '>' 10).fetch();
```

* accessing the list of connected (usually on the logic client type):

```javascript
   // Sends a message to all connected players.
   node.game.pl.each(function(p) {
       node.say('Hi!', p.id);
   });
```

### Terminating the step: the node.done() procedure

After calling `node.done()` the following steps are evaluated: 

1. If defined, the `done` callback is triggered. If not, go to
   step 2. The `done` callback can modify the parameters passed to
   `node.done()` or prevent the step from terminating, if conditions
   are not satisfied. The `done` callback is defined as a step
   property:

```javascript
stager.extendStep('mystep', {
    // Done callback.
    done: function(value) {
        // In this example, value is equal to 10 (see the cb function).
        if (value < 0) {
            // Return false to prevent the step from ending.
            alert('Negative values not allowed');
            return false;
        }
        if (value < 10) {
            // Modify the return value if needed.
            return value * 2;
        }
        // Do nothing to keep value as it is.
    },
    // Step callback.
    cb: function() {
        // User actions omitted.
        node.done({ offer: 10 });
    }
});
```

2. If no `done` callback is defined, or if one is defined and it does
not return false, the steps ends.

3. The parameters passed to `node.done()` (possibly modified by the
`done` callback) are sent to the server inside a _done_ message (below
more details).

4. The state of the client is marked `DONE`, and no other action is
possible in this step.

5. Depending on the [synchronization rule](Synchronization-v5) of the
step, the game will enter into the next step or wait for all the
players to be done (default).


#### Done messages

The _done_ message is sent from a client and stored in the internal
database of the logic. It contains the parameters passed to
`node.done()` and other important information such as the player id,
the time passed within the step, and if a timeup event happened.


```javascript
// Client side (player.js).
node.done({ offer: 100 });

// Server side (logic.js).
// The message was saved into the memory and can be accessed.
node.game.memory.select('done').first();

{
    // Time from the beginning of the step. 
    time: 10313,
    //  Flag indicating if timer for the step expired.
    timeup: false,
    // Flag indicating that this is a DONE message. 
    done: true, 
    // The sender of the message.
    player: "f18a4b6560d145008e6f951154d74d86",
    // The stage when the DONE message created.
    stage: {
                stage: 1,
                step: 1,
                round: 1
            },
    // The parameter passed to node.done
    offer: 100,
    // A unique timestamp when the message was inserted in the database.
    timestamp: 1440000935392
}
```

#### Special Cases

In case multiple parameters are passed, as in:

```javascript
node.done(param1, param2, param3);
```

They will be sent in separate done messages. To keep the same
behavior, the `done` callback must return an array. To send an array
from the `done` callback, encapsulate it inside an object.

For retro compatibility, if the `done` callback returns TRUE, the value
is ignored. This will change in future versions of nodeGame.

### Timeup events

If a timer is defined as a step property (or in the
[settings](Settings-and-Treatments-v5)) when it expires it will invoke
the `timeup` function. The default `timeup` function invokes
`node.done()` and triggers the procedure above.


### Specifying a default callback

Notice that if the callback property (`cb`) of a step is not defined
nor extended, the default callback is applied. The default callback
just prints the name of the step, does nothing.

It is possible to overwrite the default callback with a custom one:

```javascript
stager.setDefaultCallback(function() {
    console.log('I am the new default callback!');
});
```

The default callback is inserted into all steps that did not define one when the
stager gets finalized (when the game runs or by calling `stager.getState()`)

## Next Topics

* [Look at concrete examples of how to exchange data between players and server](Data-Exchange-Examples-v5) 
