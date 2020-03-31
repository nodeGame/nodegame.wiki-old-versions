- status: complete
- version: 3.x
- follows from: [Game Advanced](Game-Advanced-v3)

NodeGame supports multiple games at the same time. Each game is
executed in a separate _channel_. The channel has the same name of the
game, so if your game is called _myexperiment_, the following address
will be reserved to join the experiment:

    http://localhost:8080/myexperiment/


The following files are available:

* channel.settings.js
* channel.secret.js
* channel.credentials.js

### File: channel.settings.js

Channel's default options usually will do just fine for most users.
However, if in need, `channel/channel.settings.js` file will allow you
to configure the channel to a very low level of detail.

Each channel is divided into two internal servers: 'player' and
'admin.' Each server has a different endpoint, and connecting to it
grants different privileges in what kind of messages and commands can
be executed. The endpoint is an address to which socket.io will
connect from the index file of the game.

The options for the player and admin server are specified inside the
`playerServer` and `adminServer` properties. However, if both servers
share the same options, then there is no need to declare these
objects. In this case, the default endpoints will be used: the name of
the channel (for player server), and the name of channel /admin (for
admin server). 

However, if only the endpoint is different between server and admin
server, then `playerServer` and `adminServer` can be specified as
string containing the names of the endpoints.

If `playerServer` and `adminServer` are specified as objects, then
property _endpoint_ contains the endpoints for each server.

An example of configuration object follows:

```javascript
module.exports = {

    // By default the name of the channel is the name of the game,
    // as found in the package.json file. This means that files will
    // be served from the address http://myserver/gameName/
    // Here you can add aliases to enable urls like: http://myserver/alias/\
    //
    // Important! `node.connect()` (usually in `public/js/index.js`) needs 
    // to use the real channel name, so you might need to pass it explicitly:
    // `node.connect('/gameName').
    //
    // alias: [],

    // Name of the endpoint for socket.io player connections.
    //
    playerServer: 'mygame',

    // Name of the endpoint for the socket.io admin connections.
    //
    adminServer: 'mygame/admin',

    // All options below are shared by player and admin servers.

    // If TRUE, players can invoke GET commands on admins.
    //
    getFromAdmins: true,

    // Unauthorized clients will be redirected here.
    // (defaults: "/pages/accessdenied.htm")
    //
    accessDeniedUrl: 'unauth.htm',

    // By default player actions are notified to admins only. Force
    // notification to other players connected in the same game room:
    //
    notify: {

        // When a player connects / disconnects.
        onConnect: false,

        // When a player changes a stage / step.
        onStageUpdate: false,

        // When the 'LOADED' stageLevel is fired (useful to sync players)
        onStageLoadedUpdate: false,

        // When any change of stageLevel happens (e.g. INIT, CALLBACK_EXECUTED)
        // Notice: generates a bit overhead of messages.
        onStageLevelUpdate: false,

    },

    // If TRUE, only one TAB per browser will be allowed.
    //
    enableReconnections: true,

    // If TRUE, only reconnections in the same step are allowed.
    //
    sameStepReconnectionOnly: false,

    // If TRUE, a reconnection that fails for any reason is disposed,
    // instead of being treated as new connection.
    //
    disposeFailedReconnections: false,
    

    // The duration in ms of the browser cache for public/ resources
    // Default: 0 (no cache)
    //
    cacheMaxAge: 360000,

    // If TRUE, clients connecting via Socket.io can set own parameters
    // Available parameters:     
    // - clientType: sets the client type
    // - startingRoom: sets the room in which the client will be placed first
    //
    // It is recommended to disable sioQuery in production. Default: TRUE
    //
    sioQuery: false,
    
    // If TRUE, it will be the default channel of the server.
    // All the static files will be served from '/'.
    // The route `/channelName` will be disabled, while aliases,
    // if defined, will continue to work.
    // Important! Socket.io connection must still be established
    // with the right endpoint (e.g. /channelName).
    // Important! Other games might not be reachable any more.
    // Important! Server info query will be disabled.
    //
    defaultChannel: false
};
```

<a name="sioquery"></a>
#### Channel query parameters: sioQuery

The `sioQuery` (SocketIo Query) is particularly important. It controls
"channel query parameters", which are extra parameters passed along
the url of the nodeGame server. For example, if the url to join an
experiment is:

`http://myserver.com/mygame/`

then query parameters would be attached to the end as follows:

`http://myserver.com/mygame/?clientType=autoplay&startingRoom=Room1`

There are two channel parameters that can be set:

* clientType: sets the [client type](Client-Types-v3) that will be
  assigned the connecting client.
* startingRoom: sets the initial room where the client will be
  placed. If not specified, clients are placed first in the
  requirement room, and then moved into a waiting room.


### File: channel.secret.js

Contains a secret string with whom all json web tokens are
signed. For example:

```javascript
module.exports = function(settings, done) {
    return 'this is a secret';
};
```

This file is used only if authorization is enabled.

### File: channel.credentials.js 

Contains user name and password to access the monitor interface. For example:

```javascript
module.exports = function(settings, done) { 
    return {
        user: 'admin',
        pwd: 'password'
    };    
};
```

This file is used only if authorization is enabled.

## Next

* [Requirements Checkings](Requirements-Checkings-v3)

## Go Back to 

* [Home](Home)
