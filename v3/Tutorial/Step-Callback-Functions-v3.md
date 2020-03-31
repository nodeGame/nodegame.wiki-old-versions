- status: complete
- version: 3.x
- follows from: [Client Types](Client-Types-v3)

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
a step of the game sequence using the [Stager API](Stager-API-v3).

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
[Window API](Window-API-v3) to handle the interaction of the user with
the page, and of other methods of the [Client API](Client-API-v3) to
exchange game messages and perform other game-related tasks.

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

1. When `node.done()` is called the `done` callback is triggered (if
defined). The callback receives any parameter passed to `node.done()`
and evaluates them and any other condition necessary to finish the
game. In case a condition is not satisfied, and step should not be
terminated, the `done` callback can return false to abort the
`node.done()` procedure. 

2. In case everything is OK, the `done` callback can simply exit, and
in this case every parameter passed to `node.done()` will be sent to
the server, along with data about the duration of the step, and
whether a 'TIMEUP' event occurred. If multiple parameters are
specified, separate entries will be sent.

3. The `done` callback can also have its own return value to send to
server. If multiple values need to be sent, they must be encapsulated
in an array. Any value returned by the `done` callback will overwrite
the input parameters of `node.done()`.

4. The state of the client then marked as `DONE`, and no other action
is possible in this step.

5. Depending on the [synchronization rule](Synchronization-v3) of the
step, the game will enter into the next step or wait for all the
players to be done (default).


#### Timeup events

If a timer is defined as a step property (or in the
[settings](Settings-and-Treatments-v3)) when it expires it will invoke
the `timeup` function. The default `timeup` function invokes
`node.done()` and triggers the procedure above.


#### Done messages

The _done_ message is sent from a client and stored in the internal
database of the logic. It contains any parameter that is passed to
`node.done()`, and other important information such as the player id,
the time passed within the step, and if a timeup event was
registered. For example:


```javascript
// Client side (player.js).
node.done({ offer: 100 });

// Server side (logic.js).
// The message was saved into the memory and can be accessed.
node.game.memory.select('done').first();

//   {
//      "time": 10313,
//        "timeup": false,
//        "done": true,
//        "player": "f18a4b6560d145008e6f951154d74d86",
//         "stage": {
//            "stage": 1,
//            "step": 1,
//            "round": 1
//        },
//        "offer: 100,
//        "timestamp": 1440000935392
//    }
```
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

The default callback is inserted into all steps that did not define
one when the stager gets finalized, i.e. by calling
`stager.getState()`.

## Next Topics

* [Look at concrete examples of how to exchange data between players and server](Data-Exchange-Examples-v3) 