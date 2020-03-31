- status: complete
- version: 4.x
- follows from: [Game Advanced](Game-Advanced-v4)

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

   /**
     * ## name (string) Optional
     *
     * The name of the channel
     *
     * Default: the name of the game, as found in the package.json file.
     */
    // name: '{NAME}',

    /**
     * ## alias (string|array) Optional
     *
     * Alternative name/s for the channel
     *
     * By default, if 'gameName' is the name of the channel, files will
     * be served from the address: `http://myserver/gameName/`.
     * Here you can add aliases to enable urls like: `http://myserver/alias/`.
     *
     * Important! `node.connect()` in `public/js/index.js` still needs
     * to use the real channel name, so you might need to pass it explicitly:
     * `node.connect('/gameName').
     */
    // alias: [],

    /**
     * ## playerServer (object|string) Optional
     *
     * Set of custom options applying only to player server
     *
     * If string, it will be interpreted as the name oof the server
     * endpoint for socket.io player connections.
     *
     * If object, the endpoint must be specified in the _endpoint_ property.
     *
     * Default: name-of-the-channel
     */
    // playerServer: '{NAME}',

    /**
     * ## adminServer (object|string) Optional
     *
     * Set of custom options applying only to admin server
     *
     * If string, it will be interpreted as the name oof the server
     * endpoint for socket.io admin connections.
     *
     * If object, the endpoint must be specified in the _endpoint_ property.
     *
     * Default: name-of-the-channel/admin
     */
    // adminServer: '{ADMIN}',

    /**
     * ## getFromAdmins (boolean) Optional
     *
     * If TRUE, players can invoke GET commands on admins
     *
     * Default: FALSE
     */
    getFromAdmins: true,

    /**
     * ## accessDeniedUrl (string) Optional
     *
     * Unauthorized clients will be redirected here.
     *
     * Default: "/pages/accessdenied.htm"
     */
    // accessDeniedUrl: 'unauth.htm',

    /**
     * ## notify (object) Optional
     *
     * Configuration options controlling what events are notified to players
     *
     * Default: player actions are notified to admins only.
     */
    notify: {

        // When a player connects / disconnects.
        onConnect: false,

        // When a player changes a stage / step.
        onStageUpdate: false,

        // When the 'LOADED' stageLevel is fired (useful to sync players)
        onStageLoadedUpdate: false,

        // When any change of stageLevel happens (e.g. INIT, CALLBACK_EXECUTED)
        // Notice: generates a lot of overhead of messages.
        onStageLevelUpdate: false,

    },

    /**
     * ## enableReconnections (boolean) Optional
     *
     * If TRUE, only one TAB per browser will be allowed
     *
     * Default: FALSE
     */
    enableReconnections: true,

    /**
     * ### sameStepReconnectionOnly (boolean) Optional
     *
     * If TRUE, only reconnections in the same game step are allowed.
     *
     * Default: FALSE
     */
    // sameStepReconnectionOnly: false,

    /**
     * ### disposeFailedReconnections (boolean) Optional
     *
     * If TRUE, failed reconnections are disposed
     *
     * If FALSE, failed reconnections are treated as a new connection.
     *
     * A reconnection can fail for the following reasones:
     *
     * - Parameter `enabledReconnections` is FALSE.
     * - Parameter `sameStepReconnectionOnly` is TRUE, and the
     *      client's stage and the logic's stage are different.
     * - The room in which the client was at the moment of
     *      disconnection cannot be located.
     * - Any other error.
     *
     * Default: FALSE
     */
    // disposeFailedReconnections: true,

    /**
     * ### cacheMaxAge (number) Optional
     *
     * The duration in ms of the browser cache for public/ resources
     *
     * Default: 0 (no cache)
     */
    // cacheMaxAge: 360000,

    /**
     * ### sioQuery (boolean) Optional
     *
     * If TRUE, clients connecting via Socket.io can set own parameters
     *
     * Available parameters:
     *
     *  - clientType: sets the client type
     *  - startingRoom: sets the room in which the client will be placed first
     *
     * It is recommended to disable sioQuery in production
     *
     * Default: TRUE
     */
    // sioQuery: false,

    /**
     * ### defaultChannel (boolean) Optional
     *
     * If TRUE, the game is served from / (instead of /gamename/)
     *
     * The route `/gamename` will be disabled, while aliases,
     * if defined, will continue to work if not shadowed by any public path.
     *
     * Important! Socket.io connection must still be established
     * with the right endpoint (e.g. /channelName).
     *
     * Important! Other games might not be reachable any more.
     *
     * Important! Server info query will be disabled.
     *
     * Default: false
     */
    // defaultChannel: false,

    /**
     * ### noAuthCookie (boolean) Optional
     *
     * If TRUE, a cookie is set even with authorization disabled
     *
     * Opening multiple browser tabs will cause a disconnection in other ones.
     *
     * Default: false
     */
    // noAuthCookie: false,

    /**
     * ### roomOwnDataDir (boolean) Optional
     *
     * If TRUE, each new room in the channel has an own data dir named after it
     *
     * Default: true
     */
    // roomOwnDataDir: true,

    /**
     * ### roomCounter (number) Optional
     *
     * If set, room counter is initialized to this value
     *
     * If undefined, room counter self-initialize to the next available id,
     * starting from 1, as found in the data folder of the game.
     *
     * Default: undefined
     */
    // roomCounter: 100,

    /**
     * ### roomCounterPadChars (0 <= number <= 12) Optional
     *
     * If set, leading 0 are added to the room counter to reach desired length
     *
     * For example, if `roomCounterChars` is equal to 6 and
     * the current roomCounter value is 123, then room name is: '000123'.
     *
     * Default: 6
     */
    // roomCounterPadChars: 6    
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

* clientType: sets the [client type](Client-Types-v4) that will be
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

* [Requirements Checkings](Requirements-Checkings-v4)

## Go Back to 

* [Home](Home)
