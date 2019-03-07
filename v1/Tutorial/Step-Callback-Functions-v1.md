- status: complete
- version: 1.x
- follows from: [Client Types](Client-Types-v1)

## Overview

Step callback functions are executed when a client enters a new step
of the game sequence.

They are stored in the `cb` property of the step objects and defined
in the client types files.

## Implementation

A step callback must prepare the step to played, and flag it as _done_
when certain conditions are met.

### Preparing the step: the node object, and the W object

The preparation of the step requires accessing methods of the `node`
object which is available inside the step callback. When the step
callback is executed in the browser, additionally the `node.window`
object is available (aliased to the global `W` object), exposing
methods to manipulate the browser's window.

What to do here really depends on what the game designer is trying to
achieve, and on the client type (`player` or `logic`).

However, common actions are as follows:

* loading a new frame (only in the browser):
  
```javascript
W.loadFrame('instructions.htm', function() {
   // Execute this code after 'instructions.htm' 
   // was loaded asynchronously.
});
```

* sending game messages: 

```javascript

    // node.say

    // Sends a message labeled 'Hello!' to the game room's logic.
    node.say('Hello!');
    // Send a message labeled 'offer' to a certain receiver,
    // with a data value equal to 100.
    node.say('offer', receiverId, 100);
    
    //node.get
        
    // Send a message labeled 'question' to a certain receiver,
    // and when the reply is obtain executes the callback.
    node.get('question', receiverId, function(reply) {
        // Execute this code with the reply from the receiver.
    });
    
    // node.set
    
    // Stores a object in the logic database.
    node.set({
        offer: 100,
        role: 'bidder',
        time: new Date().getTime()
    });
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


   // Listen to incoming 'get' messages.
   // 'get' messages are emitted locally if authorized.
   // The return value of the callback function is then sent
   // back to the sender of the 'get' message.
   
   node.on('get.question', function(msg) {
       return 'My answer is 10';
   });   
```

Important! An event listener registered inside the step callback
function will always catch all the events generated by incoming
messages in that step. In fact, messages arriving before the execution
of the step callback function is terminated (including asynchronous
loading of a frame) are buffered and emitted only afterward.

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

### Terminate the step: node.done()

The step callback must also listen to the action or condition that
makes the step _done_. For example, a step is _done_ if the user
clicks a certain button on screen, or a specific game message is
received from the server, or a timeout expires, etc. When this happens
`node.done()` must be invoked. For example:

```javascript
// Assuming we have a reference to a button.
button.onclick = function() {
   node.done();
};
```

When `node.done()` is called the state of the client is marked as
`DONE`, and no further actions are possible in this step. A message is
automatically sent to the game room _logic_ client type, communicating
the change of state. When all the players are done, the _logic_
communicates to all the players to execute the next step.

The _done_ message is stored in the internal database of the logic,
together with any parameter that are passed along, and other important
information such as the player id, the time passed within the step,
and if a timeup event was registered. For example:


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

## Next Topics

* Next: [Game Advances](Game-Advanced-v1)