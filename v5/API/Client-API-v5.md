- status: **NEEDS UPDATES**
- version: 5.x

## Introduction

The _Client API_ library exports the `node` object. It contains methods to:

- Setup the node instance.
- Connect to a nodeGame server.
- Create and exchange game messages.
- Setup, start, pause, resume, and stop the execution of a game.
- Log info, warn, and error messages.
- Measure time between events.
- Manipulate objects in the memory database.
- Define a game sequence.

The _Client API_ is essentially the same in the browser and in Node.js. The browser API has the additional methods to manage the user interface.

## API Summary

### Loading the API

In the browser:

```html
// Add socket.io for communication.
<script src="/socket.io/socket.io.js"></script>
// Load the full library.
<script src="/javascript/nodegame-full.js" charset="utf-8"></script>
```

In Node.js:

```javascript
const ngc  = require('nodegame-client');
let node = ngc.getClient();
```

### First-level properties and objects

A newly instantiated instance of the _Client API_ exposes the
following properties and objects:

- `node.game`<br>
  The game being played. It is the most important object of the API, and it controls and executes all the stages and steps of the [game](Game-API-v5).
- `node.widgets`<br>
  Creates and manages the [widgets](Widgets-v5).
- `node.window`<br>
  Manages the user interface. It is an alias to the [Window object](Window-API-v5).
- `node.msg`<br>
  The msg generator to create custom [game messages](Game-Messages-v5).
- `node.events`<br>
  The event manager. Registers a executes [event listeners](Event-Listeners-v5).
- `node.socket`<br>
  The connection socket to the server.
- `node.player`<br>
  The player information about the client.
- `node.conf`<br>
  The configuration values used by setup functions.
- `node.timer`<br>
  The timer object to create new timers and handle
  [timing events](Timer-API-v5).
- `node.JSUS` <br>
   Collection of helper functions from the [JSUS package](https://github.com/nodeGame/JSUS), including: randomization and sorting, Latin squares, parsing, and manipulating objects and arrays. Note: in the browser you
   can simply use the `J` reference.


### Connecting to a server

- `node.connect(channel, conf)`<br>
  Connects to the specified channel endpoint.
  It accepts extra configuration options for the socket. If no
  parameter is specified, it will try to guess the name of the channel
  based on the uri of the page.

#### Examples

  ```js
  // Connect to the ultimatum endpoint (leading slash needed).
  node.connect('/ultimatum');
  ```

### Emitting and catching events locally

- `node.on('KEY', callback)`<br>
  Registers an event-handler callback that will be executed _each time_ the
  event 'KEY' is emitted.
 - `node.once('KEY', callback)`<br>
  Registers an event handler callback that will be executed only the first
  time the event 'KEY' is triggered.
- `node.emit('KEY', params)`<br>
  Emits the event 'KEY' locally.
  Accepts any number of additional parameters.
  All registered event handlers on 'KEY' will be executed.
- `node.off('KEY', callback)`<br>
  Removes all event handlers for event 'KEY', or just the specified callback.

 _Important!_
The emit method by itself does **NOT** send data to other players or the
server. However, emitting particular types of events locally triggers other
 event listeners that in turn send the data to the server or other players. Here is a list of existing [events](Events-v5).

#### Examples

Registers a new event local event listener that increments a local variable with the value received by the emitting function; the current value of the local variable is returned back to the emitting function.
```js
var localData = 1;
node.on('INCREMENT_DATA', function(increment) {
    // Update a local variable.
    localData += data;
    // Return a value to the emit function.
    return localData;
});

node.emit('INCREMENT_DATA', 1); // returns 2.
```

In case there are more than one event listeners registered, they are executed in the order they are registered. The return values are returned inside an array.

```js
var localData = 1;
node.on('INCREMENT_DATA', function(increment) {
    // Update a local variable.
    localData += data;
    // Return a value to the emit function.
    return localData;
});
node.once('INCREMENT_DATA', function(increment) {
    // Update a local variable.
    localData += data;
    // Return a value to the emit function.
    return localData;
});

// First call, two event listeners registered.
node.emit('INCREMENT_DATA', 1); // returns [2, 3].
// Second call, after one event listener is removed.
node.emit('INCREMENT_DATA', 1); // returns 4.
```

### Sending events between clients


- `node.say('KEY', receiver, payload)`<br>
  Emits an event 'KEY' on a remote node object (receiver).
  Can send an optional payload along with the event.
- `node.set('KEY', value, to)`<br>
  Saves a key-value pair into the memory of the node object of the logic of the
  game room.
  Optionally, the pair can be stored in another node object.
- `node.get('KEY', callback, receiver, params, timeout)`<br>
  Emits the event 'KEY' on a remote node object (receiver); register a local
  event handler that will be executed when return value from the remote
  invocation is received.  
- `node.on.data('KEY', callback)`<br>
  Registers an event handler callback that will be executed when an incoming
  _DATA_ message with the specified 'KEY' label is received.


When a `node` instance receives an incoming game message, the event
_in.[say|get|set].[target]_ is fired with the incoming message as
input parameter.

#### Examples

```javascript

// Sending the _DATA_ message previously created to 'receiver'.
node.say('myData', 'receiver', [ 0, 1, 2, 3 ]);

// 'receiver' node instance handle the incoming _DATA_ message
node.on('in.say.DATA', function(msg) {
   if (msg.text === 'myData') {
       // Do something with the data.
       console.log(msg.data); // [ 0, 1, 2, 3 ]
   }
});
```

Alternatively, it is possible to use the shortcuts to send and receive game
messages.

```javascript
// Sending the _DATA_ message previously created to 'receiver'.
node.say('myData', 'receiver', [ 0, 1, 2, 3 ]);

// 'receiver' node instance handle the incoming _DATA_ message
node.on.data('myData', function(msg) {
   // Do something with the data.
   console.log(msg.data); // [ 0, 1, 2, 3 ]
});
```

It is also possible to execute remote procedure calls on remote `node`
instances.

```javascript
// On remote instance register an handler for "get.myData" events.
node.on('get.myData', function(msg) {
    return [ 0, 1, 2, 3 ];
});

// Sending a  _get.DATA_ message to 'receiver',
// and handling the response message.
node.get('myData', 'receiver', function(response) {
    // Do something with response.
    console.log(response); // [ 0, 1, 2, 3 ]
});
```

### Aliases

- `node.alias('myalias', event, modifier)`<br>
  Creates a shortcut for registering event handlers for specific events.
  The alias will be available under node.on.myalias (for example,
  `node.on.data` is an alias for both 'in.say.DATA' and 'in.set.DATA' messages).

### Setting up a node instance

- `node.registerSetup('feature', func)`<br>
  Register a setup function available under node.setup.feature.
  All setup functions can be invoked remotely sending a _SETUP_ message.
- `node.setup('feature', conf)`<br>
  Calls a setup function locally with the required configuration.
  All return values are stored under node.conf.


For the all configuration options accepted by `node.setup` refer to
 the [configuration guide](Client-Configuration-v5).

After invoking `node.setup`, all settings are stored in the
`node.conf` object.


### Controlling the Console Output

The amount of text logged to console is controlled by the parameter
`node.verbosity`.
The parameter can be set directly, via `node.setup.verbosity(level)`, or
remotely via `node.remoteSetup('verbosity', level)`.
The verbosity parameter is a number usually between -1 and 100.

The level can be encoded as a string compatible with
[winston.js](https://github.com/flatiron/winston) levels.

```javascript
ALWAYS: -Number.MAX_VALUE,
error: -1,
warn: 0,
info: 1,
silly: 10,
debug: 100,
NEVER: Number.MAX_VALUE
```

The method `node.log(text, level)` prints to console only log messages with a
level higher or equal to the `node.verbosity`.
Other shortcut methods are available to log messages with a pre-defined log
level.

```javascript
node.log(text, level, prefix);
node.info(text, prefix);
node.warn(text, prefix);
node.error(text, prefix);

// The prefix is added automatically.
// to the log message. By default the prefix includes
// the node.nodename variable and the error level.
```

### Admin Commands

Some commands are available only to `node` clients with higher
privileges.  They allow to setup, and execute commands on remote
`node` instances.

- `node.remoteSetup(feature, to)`<br>
  can setup remotely any of the features that `node.setup()` locally.
  Keep in mind that  some features might be locked when a game is running.
- `node.remoteCommand(command, to, options)`<br>
  Executes a game command on a remote client. Available game commands are:
  - 'start'
  - 'pause'
  - 'resume'
  - 'stop'
  - 'restart'
  - 'step'
  - 'push_step'
  - 'goto_step'
  - 'clear_buffer'
  - 'erase_buffer'
- `node.remoteAlert(command, to)`<br>
  Displays an alert message in the screen of the
  remote client. If the client is a bot, the message will be printed to console.
- `node.redirect(url, who)`<br>
  redirects a remote client to the specified URL.
  If the client is a bot the message will be ignored.

## Additional Client API pages

- [Stager API](Stager-API-v5)
- [Matcher API](Matcher-API-v5)
- [Client API](Client-API-v5)
- [Game API](Game-API-v5)
- [Timer API](Timer-API-v5)
- [Window API](Window-API-v5)
