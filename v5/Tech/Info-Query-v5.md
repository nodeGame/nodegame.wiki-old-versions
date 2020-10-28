- status: complete
- version: 5.x


The Info Query allows for the extraction of information about the state of the server via a REST API call.

### Usage and Examples

Assuming your server runs on localhost and port 8080, you can execute an info query accessing the following url:

```
http://localhost:8080?q=PARAMETER
```

where `PARAMETER` can take any of the following values:

#### channels

Returns high-level information for every active channel.

_Call:_

```
http://localhost:8080?q=channels
```

_Return value:_

```js
{
    "ultimatum": {
        "sockets":{"sio": true, "direct": true },
        "accessDeniedUrl":"/ultimatum/unauth.htm",
        "verbosity":100,
        "getFromAdmins":true,
        "enableReconnections":true,
        "defaultChannel":false,
        "name":"ultimatum",
        "roomCounter":2,
        "gameName":"ultimatum",
        "open":true
    }
}
```

#### games

Returns detailed information about the game played in every channel.

_Call_:

```
http://localhost:8080?q=games
```

_Return value:_

```js
{
    "ultimatum": {
        "dir": "path/to/dir/game",
        "info": { }, // Content of package.json
        "settings": { // As in game.settings.js
            "standard": { },
            "pp": { }
        },
        "treatmentNames": ["standard","pp"],
        "setup": { },            // As in game.setup.js
        "stager": { },           // Game sequence.
        "languages":{ },         // Any languages supported.
        "channel": { },          // As in channel.settings.js
        "waitroom":{ },          // As in waitroom.settings.js
        "auth":{ },              // As in auth.settings.js
        "requirements":{ },      // As in requirements.settings.js
        "aliases":{}             // If any defined
    }
}
```

#### info

Returns combined info for active games and channels.

_Call:_

```
http://localhost:8080?q=info
```

_Return value:_

```js
{
    "channels": {
        "ultimatum": {  } // See ?q=channels
    }
    "games": {
        "ultimatum": {  } // See ?q=games
    }
}
```

#### waitroom (_v5.10.1+_)

Returns current status of waiting rooms.

_Call:_

```
http://localhost:8080?q=waitroom&game=ultimatum
```

_Return value:_

```js
{"nPlayers":0} // Players currently waiting.
```

### Enabling Info Query

The Info query is disabled by default. To enable the server configuration (`conf/servernode.js`) file inside the nodeGame installation folder and add the following line:

```js
servernode.enableInfoQuery = true;
```

### Cyclic Structures

In case of cyclic structures (e.g., a loaded database in game.setup.js) an error will be raised.

## Go Back to

* [Home](Home)
