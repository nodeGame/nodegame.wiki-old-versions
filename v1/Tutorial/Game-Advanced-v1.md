- status: complete
- version: 1.x
- follows from: [Game Basics](https://github.com/nodeGame/nodegame/wiki/Game-Basics-v1#directory_structure)

## Directory Structure

When you create a new game with the nodegame-generator utility the
game folder will contain the following sub-folders and files:

* game/
* public/
* waitroom/
* **auth/**
* **channel/**
* **data/**
* **requirements/**
* **views/**
* **package.json**

In this advanced guide we focus on the items in bold. The other items
were covered in the [Game Basics](Game-Basics-v1) page.

### auth/ folder

The files in this folder define the authorization requirements to
access the game channel.

The following files are available:

* auth.settings.js
* auth.js
* auth.codes.js

#### auth.settings.js

This file exports an object containing the authorization settings. To
enable authorization for your channel. Just set:

    enabled: true
    
If authorization is enabled, then the list of valid authorization
codes must be provided in the `codes` property. It can contains
directly an array of codes objects, or the path to the codes file,
exporting a function returning the array of codes. The default name
codes file is "auth.codes.js".

If authorization is enabled, players must connect to the address:

    http://localhost:8080/myexperiment/auth/id/password 

assuming your experiment is called _myexperiment_

Notice that the password parameter can be omitted if it not specified
in the codes array (see below).

All the variables in this file are then passed as a parameter of the
function exported in the codes file. The property `mode` controls how
the codes are generated in the default code function.

#### auth.codes.js

If authorization is enabled in auth.settings.js, and the default
`codes` option is not overridden, then this file must return a
function that (synchronously or asynchronously) returns an array of
code objects. 


A code object contains an id field and optionally and pwd field. For
example:

```javascript
{ 
    id: 'foo', 
    pwd: 'optional_password'
} 
```

All the codes returned by the codes function will be loaded into the
channel registry and will be available as long as the server is
running. Notice that any additional property (besides `id`, and `pwd`)
will also be added to the codes objects in the registry.

The code function accepts two parameters: an object with the
authorization settings as in auth.settings.js file, and a _done_
function to return the array of codes asynchronously. For example:

```javascript
    loadCodesFromDatabase(function(err, codes) {
        // Codes are loaded, return them.
        done(err, codes);
    });
```

#### auth.js

This file allow to define a number of callbacks to customize the
authorization mechanisms of your channel. The file must export a
function that accept a configuration object as input parameter.

The configuration object exposes three methods: `authorization`,
`clientIdGenerator` and `clientObjDecorator`.

Each of these methods accepts 1 or 2 parameters; the last one must be
a callback function and the first one is the name of the server
('player' or 'admin') to which the callback should be applied to. If
only the callback is provided, then it is applied to both 'player' and
'admin' servers.


* **authorization**: The function must return true to authorize the
    connection. It accepts to input parameters: a reference to the
    channel object, and an object containing information about the
    newly connecting client:

```javascript
{
   headers: Info about connections
   cookies: Cookies passed on connections
   room: The requested room for connection, null otherwise
   clientId: If specified by the signed token, null otherwise
   clientType: The client type: e.g. 'player', 'bot', ...
   validSessionCookie: TRUE if the channel session is matched
}
``` 

* **clientIdGenerator**: The function must return a string
    representing the id for the connecting client, or undefined to
    fall back on the default id assignment mechanism. It accepts two
    parameters as input, exactly as for the authorization function.


* **clientObjDecorator**: The function accepts two parameters as
    input: the first one is current client object, and second one is
    the info object, as described in the authorization function. The
    properties of the client object depend on the server
    configuration. Modification to the client object will be
    immediately effective, however, some properties can never be
    modified, or an error will be thrown: id, sid, admin, clientType.


### channel/ folder

The files in this folder configure the technical aspects of the game
channel. NodeGame supports multiple games at the same time. Each game is
contained in a separate folder, and it is assigned an own _channel_. A
channel has the same name of the game, so if your game is called
_myexperiment_, the following address will be reserved to join the
experiment:

    http://localhost:8080/myexperiment/


The following files are available:

* channel.settings.js
* channel.secret.js
* channel.credentials.js

#### channel.settings.js

Channel's default options usually will do just fine. However, the
channel.settings.js file will allow you to configure the channel to a
very low level of detail.

Each channel is divided into two internal servers: 'player' and
'admin.' Each server has a different endpoint, and connecting to it
grants different privileges in what kind of messages and commands can
be executed. The endpoint is an address to which socket.io will
connect from the index file of the game.

The options for the player and admin server are specified inside the
`playerServer` and `adminServer` properties. However, if both servers
share the same options, then just a string with the name of the two
different endpoints must be specified.

If different options apply to each server, `playerServer` and
`adminServer`, then they must be specified as configuration objects,
and the name of the _endpoint_ specified as a key in the object.

An example of configuration object follows:

```javascript
module.exports = {

    // By default the name of the channel is the name of the game,
    // as found in the package.json file. This means that files will
    // be served from the address http://myserver/gameName/
    // Here you can add aliases to enable urls like: http://myserver/alias/
    // alias: [],

    // Name of the endpoint for socket.io player connections
    playerServer: 'mygame',

    // Name of the endpoint for the socket.io admin connections
    adminServer: 'mygame/admin',

    // All options below are shared by player and admin servers.

    // Default verbosity for node instances running in the channel,
    // e.g.: logics, bots, etc.
    verbosity: 100,

    // If TRUE, players can invoke GET commands on admins.
    getFromAdmins: true,

    // Unauthorized clients will be redirected here.
    // (defaults: "/pages/accessdenied.htm")
    accessDeniedUrl: 'unauth.htm',

    // By default player actions are notified to admins only. Force
    // notification to other players connected in the same game room:
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
    enableReconnections: true

};
```


#### channel.secret.js

Contains a secret string with whom all json web tokens are
signed. This file is used only if authorization is enabled.

#### channel.credentials.js 

Contains user name and password to access the monitor interface. This
file is used only if authorization is enabled.

### requirements/ folder

The files in this folder specify the technical requirements for
accessing the channel. Some basic features of the browser are checked,
as well as the connection speed. It is possible to specify custom
requirements as well.

The following files are available:

* requirements.settings.js
* requirements.room.js
* requirements.js

#### requirements.settings.js 

Contains the settings for the requirements checkings. At the moment,
the only two options available are whether the requirement room is
`enabled` (true or false), and whether a custom requirements room
should be used (`logicPath`).


#### requirements.room.js 

Contains the code for the requirements room. If you are OK with the
default room, you should not change this file.


#### requirements.js 

Contains the list of requirements being evaluated on the connecting
client machines. This file must export a function that takes as input
two parameters:

 - the `requirements` object, and
 - the requirements settings, as defined in requirements.settings.js 

The `requirements` object exposes the following methods:

  - `.add(function)`: adds a requirements callback which will be
    evaluated on the connecting client. The function must return an
    object of the type:

         { success: false|false, errors: ['err 1', 'err2', ... ] };

    The requirement callback can also return asynchronously, by
    calling the _result_ function passed as first input parameter
    
  - `.onFailure(function)`: adds a callback function which will be
    executed on the client machine if at least one test fails.
    
  - `.onSuccess(function)`: adds a callback function which will be
    executed on the client machine if at least no test fails.
 
 - `.onComplete(function)`: adds a callback function which will be
    executed after all tests have been executed, regardless of the
    result.
  
  - `setMaxExecutionTime(number)`: sets the maximum execution time for
    all tests.

### data/ folder

In this directory is where the results of the experiments are supposed
to be saved. The experimenter must specify what to save.

### views/ folder

Views are dynamically created HTML pages. The files in this folder are
divided in two sub-folders:

* templates/
* contexts/

Templates are [jade](http://jade-lang.com/) files which define the
structure of the page. Contexts contain the variables used to fill in
the content of the templates.

For a full guide about how to create views see the [Views](Views-v1)
page.

### package.json

This file contains general information such as the name of the game,
its version and the name of author. The same file is used by [npm](
https://www.npmjs.com/) for packaging the game as a publishable module.


```javascript
{
    "name": "ultimatum",
    "version": "0.1.0",
    "description": "This is an ultimaum game",
    "author": "Author Name <author@email.com>",
    "license": "MIT/X11",
    "homepage": "http://nodegame.org"
}
```

## Next

* [Synchronization](Synchronization-v1)

## Go Back to 

* [Home](Home)
- [Game Basics](https://github.com/nodeGame/nodegame/wiki/Game-Basics-v1#directory_structure)
