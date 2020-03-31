- status: incomplete
- version: 3.x

**Ehm...sorry, this wiki page is under construction!**

## Overview

All logic [client types](Client-Types-v3) have access to the _Game
Room_ object in which they are instantiated. This object, in turn,
contains a reference to the _Channel_ object in which it is embedded.


### Bots and Phantoms

There are two types of bot clients that can be set up to play a game, either as
a replacement for human players or for testing purposes.

The first kind, instantiated using `ServerChannel.connectBot`, resides directly
on the server, giving it a SocketDirect connection and thus admin privileges.

The second kind, created by `ServerChannel.connectPhantom`, uses PhantomJS to
emulate a browser connection.
This way, the environment of the bot is very similar to that of a human player
which makes it useful for testing.

#### Bots

To connect a bot of [client type](Client-Types-v3) equal to 'bot' to
the [waiting room](Waiting-Room-v3):

```javascript
channel.connectBot();
```

To connect a custom bot and place it in any given room, with special
[setup](Client-Configuration-v3) configuration:

```javascript
channel.connectBot({
    room:       'roomId', // Or room object itself.
    clientType: 'bot2',
    setup: {
       // Add extra setup options here.
       settings: { foo: 'bar' } // node.game.settings.foo = 'bar'.
    }
});
```

#### Phantoms

To connect a phantom of [client type](Client-Types-v3) equal to
'autoplay' to the [requirements room](Requirements-Checkings-v3):

```javascript
channel.connectPhantom();
```

To connect a custom phantom with or without authorization:

```javascript
channel.connectPhantom({
    // Sets custom client type.
    clientType: 'phantom',
    // Passes additional parameters to the query string.
    queryString: 'foo="bar"',
    // Passes authorization credentials (id only).
    auth: 'myId',
    // Passes authorization credentials (id and password).
    auth: {
        id: 'myId',
        pwd: 'myPassword'
    },
    // Creates new valid credentials automatically.
    auth: 'createNew',
    // Uses the next available credentials (if any).
    auth: 'nextAvaiable'    
});
```

See the documentation of [ServerChannel](
https://nodegame.github.io/nodegame-server/docs/lib/ServerChannel.js.html)
for a full description of the two methods.

### Server Messages

The nodeGame servers can be controlled and queried via messages.

Some of the commands accepted by the AdminServer include:

- `SERVERCOMMAND_ROOMCOMMAND`<br>
  Instructs clients in a room to be setup, started, paused, resumed or stopped.
- `SERVERCOMMAND_INFO`<br>
  Queries the server for information about channels, rooms, clients or games.
- `SERVERCOMMAND_STARTBOT`<br>
  Starts a PhantomJS instance connected to the server, using clientType
  'autoplay'.

For more information, refer to the implementation of [AdminServer](
https://github.com/nodeGame/nodegame-server/blob/master/lib/servers/AdminServer.js
).

Examples:

```javascript
// Pause all players in a given room.
node.socket.send(node.msg.create({
    target: 'SERVERCOMMAND',
    text:   'ROOMCOMMAND',
    data: {
        type:    'PAUSE',
        roomId:  roomId,  // roomId must be defined
        doLogic: false,
        clients: 'players',
        force:   false
    }
}));

// Start a PhantomJS bot.
node.socket.send(node.msg.create({
    target: 'SERVERCOMMAND',
    text:   'STARTBOT'
}));

// Get a list of channels.
node.socket.send(node.msg.create({
    target: 'SERVERCOMMAND',
    text:   'INFO',
    data: {
        type:      'CHANNELS',
        extraInfo: true
    }
}));
// Listen for the response.
node.on.data('INFO_CHANNELS', function(msg) {
    Object.keys(msg.data).forEach(function(channel) {
        console.log(msg.data[channel].name);
    });
});
```

