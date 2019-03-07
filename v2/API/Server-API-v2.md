- status: incomplete
- version: 2.x

## Server Messages

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

## Bots

There are two types of bot clients that can be set up to play a game, either as
a replacement for human players or for testing purposes.

The first kind, instantiated using `ServerChannel.connectBot`, resides directly
on the server, giving it a SocketDirect connection and thus admin privileges.

The second kind, created by `ServerChannel.connectPhantom`, uses PhantomJS to
emulate a browser connection.
This way, the environment of the bot is very similar to that of a human player
which makes it useful for testing.

Example for the first kind:

```javascript
channel.connectBot({
    room:       room,
    clientType: 'bot',
    loadGame:   true
});
```

This will initialize a client with type 'bot' on the server, place it in the
given room and set up the game associated with that type.
The code that will be loaded for the game is looked up in the gamePaths object
in `game.settings.js`, in this case `gamePaths['bot']`.

See the documentation of [ServerChannel](
https://nodegame.github.io/nodegame-server/docs/lib/ServerChannel.js.html) for a
full description of the two methods.
