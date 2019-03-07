- status: complete
- version: 1.x

## Introduction

The `nodegame-client` library exports the `node` object.
It contains methods to:

- Setup the node instance.
- Connect to a nodeGame server through different sockets.
- Define a game sequence.
- Create, and exchange messages with other clients.
- Setup, start, pause, resume, and stop the execution of a game.
- Log info, warn, and error messages.
- Measure time between events.
- Manipulate objects in the memory database.

## In the Browser and in Node.js

The same library can be used in the browser and in Node.js with minimal
differences.
In the browser the `node` object is immediately available, while in Node.js it
must be created explicitly.

Import `nodegame-client` in an HTML page.

```html
<script src="/socket.io/socket.io.js"></script>
<script src="/javascript/nodegame-full.js" charset="utf-8"></script>
```

Import `nodegame-client` in Node.js.

```javascript
var ngc  = require('nodegame-client');
var node = ngc.getClient();
```

In Node.js `node` has special support for handling file system operations,
while in the browser it has extra methods for managing the interaction with the
browser window.

## Setting up a node Instance

The `node.setup` method allows a fine-grained
[configuration](Client-Configuration-v1).

All settings specified with `node.setup` are stored in the `node.conf` object.

## Connecting to a Server

A `node` instance must connect to [channel](Channels) of a nodeGame server in
order to communicate with other `node` instances.
This is slightly different depending on the type of socket used.
Two types of sockets are available:

- **SocketIO**: Connects to the server via HTTP, using the library
  [Socket.IO](http://socket.io).
  It used for example for the connection from a browser.
- **SocketDirect**: Connects to the server via a shared object in the server.
  It is used for example by a logic, or a bot to connect to a game room.

To connect via SocketIO the following steps are needed.

```javascript
// Connecting using SocketIO.

node.setup('host', 'myhost.com');

// Type plus other options.
node.setup('socket', {
    type: 'SocketIo',
    reconnect: false
});

node.connect('/mychannel');
```

To connect via SocketDirect the following steps are needed.

```javascript
// Connecting using SocketDirect.

node.socket.setSocketType('SocketDirect', {
    socket: channelSocketObject
});

node.connect();
```

The method `node.connect` accepts an optional second parameter that contains
options for the socket object.
Among other settings, it is possible to specify the [client type](Client-Types-v1)
and the starting room for the connection.
Such parameters should be disabled in the [configuration](Server-Configuration)
of a nodeGame server used in production.

```javascript
// SocketIO options.
var ioOptions = {
    query: 'clientType=player&startingRoom=myRoom'
};

node.connect('/mychannel', directOptions);

// SocketDirect options.
var directOptions = {
    clientType: 'player',
    startingRoom: 'myRoom'
};

node.connect(null, directOptions);
```

## Starting a New Game

nodeGame games must be defined using the [Stager API](Stager-API-v1).
Once defined, they can be executed with the following commands.

```javascript
node.setup.plot(MY_GAME);

node.game.start();
```

## Creating and Exchanging Messages

Clients communicate with other clients with [game messages](Game-Messages-v1).
All game messages are routed through the nodeGame server that checks the
permissions of the sender before delivering the message.

To create a new game message, you can use the method `node.msg.create`.
It fills automatically all defaults, and only a few parameters need to be
specified.

```javascript
// Creating a _DATA_ message.
var myMsg = node.msg.create({
    to: 'receiver',
    text: 'myData',
    data: [ 0, 1, 2, 3 ]
});

// Show all default fields.
console.log(myMsg);

GameMsg {
    id: 735605,
    session: "2614842012990266",
    from: "435250987065956",
    action: "say",
    target: "DATA",
    to: "receiver",
    text: "myData",
    data: Array[4],
    stage: GameStage {},
    created: "8-11-2014 13:28:47 898",
    forward: 0,
    priority: 0,
    reliable: 1
}
```

Once created, a message can be sent using the `node.socket.send` method which
takes the game message as the input parameter.

```javascript
// Sending the _DATA_ message previously created to 'receiver'.
node.socket.send(myMsg);
```

When a remote `node` instance receives an incoming game message, the
event _in.[say|get|set].[target]_ is fired with the incoming message
as input parameter.

```javascript
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


### NodeGame Events

`in.say.DATA` and `in.get.DATA` are two examples of _events_ available
in nodeGame. The full list of all nodeGame events to which you can
listen is available [here](Events-v1).

## Timers

In nodeGame, it is easy to create timers, fire an event at a specific
given interval, or measure the time passed between two events. For
details, refer to the [Timer API page](Timer-API-v1).


## Controlling the Console Output

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

## Admin Commands

Some commands are available only to `node` clients with higher privileges.
They allow to setup, and execute commands on remote `node` instances.

### remoteSetup

`node.remoteSetup(feature, to)` can setup remotely any of the features that
`node.setup()` locally.
Keep in mind that  some features might be locked when a game is running.

### remoteCommand

`node.remoteCommand(command, to, options)` executes a game command on a remote
client.
Available game commands are:

- 'start'
- 'pause'
- 'resume'
- 'stop'
- 'restart'
- 'step'
- 'goto_step'
- 'clear_buffer'
- 'erase_buffer'

### remoteAlert

`node.remoteAlert(command, to)` displays an alert message in the screen of the
remote client.
If the client is a bot, the message will be printed to console.

### redirect

`node.redirect(url, who)` redirects a remote client to the specified URL.
If the client is a bot the message will be ignored.

## API Summary

### Connecting to a server

- `node.connect(channel, conf)`<br>
  Connects to the specified channel endpoint.
  It accepts extra configuration options for the socket.

### Emitting and catching events locally

- `node.emit('KEY', params)`<br>
  Emits the event 'KEY' locally.
  Accepts any number of additional parameters.
  All registered event handlers on 'KEY' will be executed.
- `node.on('KEY', callback)`<br>
  Registers an event handler callback that will be executed each time the
  event 'KEY' is triggered.
- `node.once('KEY', callback)`<br>
  Registers an event handler callback that will be executed only the first
  time the event 'KEY' is triggered.
- `node.off('KEY', callback)`<br>
  Removes all event handlers for event 'KEY', or just the specified callback.

Important!
The emit method by itself does **NOT** send data to other players or the
server.
However, emitting particular types of events locally triggers other
hooks which in turn send the data out.

### Emitting and catching events over the network

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

### Aliases

- `node.alias('myalias', event, modifier)`<br>
  Creates a shortcut for registering event handlers for specific events.
  The alias will be available under node.on.myalias.
- `node.on.data('KEY', callback)`<br>
  Registers an event handler callback that will be executed when an incoming
  _DATA_ message with the specified 'KEY' label is received.

### Setup

- `node.registerSetup('feature', func)`<br>
  Register a setup function available under node.setup.feature.
  All setup functions can be invoked remotely sending a _SETUP_ message.
- `node.setup('feature', conf)`<br>
  Calls a setup function locally with the required configuration.
  All return values are stored under node.conf.

### Objects

- `node.game`<br>
  The game currently loaded.
- `node.msg`<br>
  The msg generator object.
  Can create custom game messages.
- `node.events`<br>
  The event emitter object.
  Registers a executes function listeners.
- `node.socket`<br>
  The connection socket with the server.
- `node.player`<br>
  The player data for the client.
- `node.conf`<br>
  The configuration values used by setup functions.
- `node.timer`<br>
  The timer object to create new timers and handle timing events.

### nodegame-window and nodegame-widgets

In the browser the following objects are also available:

- `node.window` or `W`<br>
  An helper object to interact with the browser window, create an header, and
  the game frame.
- `node.widgets`<br>
  A collection of reusable code snippets to specific tasks (e.g. display a
  timer, count rounds, etc.).

### Memory API

_node.game.memory_

The memory object is a special instance of the _NDDB_ database.
For more information about the _node.game.memory_ API see the
[NDDB home page](http://nodegame.github.com/NDDB/).

### JSUS

`node.JSUS` contains a collection of helper functions for diverse purpose, from
randomization and sorting, to creation of a Latin square.
See the [JSUS home page](http://nodegame.github.com/JSUS/) for the full API.
