- status: complete
- version: 2.x
- follows from: [Game Basics](https://github.com/nodeGame/nodegame/wiki/Game-Basics-v2#directory_structure)

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
* **levels/**
* **package.json**

In this advanced guide we focus on the items in bold. The other items
were covered in the [Game Basics](Game-Basics-v2) page.

### auth/ folder

The files in this folder define the authorization requirements to
access the game channel.

The following files are available:

* auth.settings.js
* auth.js

#### auth.settings.js

This file exports an object containing the authorization settings. To
enable authorization for your channel. Just set:

    enabled: true
    
If authorization is enabled, then the next variable examined is
`mode`. There are five authorization modes:

   1. **dummy**: Creates a creates dummy ids and passwords in
   sequential order. Useful for debugging purposes.
   
   2. **auto**: Creates random 8-digit alphanumeric ids and passwords.
   
   3. **local**: reads the authorization codes from a file. By
                 default, `codes.json` and `code.csv` are tried in
                 sequence. A custom file can be specified using the
                 `inFile` variable (supported formats: json and csv).
                 
   4. **remote**: fetches the authorization codes from a remote URI.
                 Supported protocol:
                 (DeSciL)[http://www.descil.ethz.ch] protocol.
                 
   5 **custom**: The `customCb` property will be executed in order to
                 get the authorization codes. The input parameters
                 are: the settings object itself, and done callback
                 for asynchronously returning the codes.

If authorization is enabled, (assuming your experiment is called
_myexperiment_) players must connect to the address:

    http://localhost:8080/myexperiment/auth/id/password 


Notice that the password parameter can be omitted if it not specified
in the codes array (see below).

##### Codes format

A code object contains an id field and optionally and pwd field. For
example:

```javascript
{ 
    id: 'foo', 
    pwd: 'optional_password'
} 
```

Notice that any additional property (besides `id`, and `pwd`) will
also be added to the code objects in the registry.


##### Authorization variables


- **nCodes**: (modes: 'dummy', 'auto') The number of codes to
    create. Default: 100
     
- **addPwd**: (modes: 'dummy', 'auto') If TRUE, a password field is
    added to each code. Default: FALSE

- **codesLength**: (modes: 'auto') The length of generated
    codes. Default: `{ id: 8, pwd: 8, AccessCode: 6, ExitCode: 6 }`

- **inFile**: (modes: 'local') The name of the codes file inside `auth/`
   directory or a full path to it. Supported formats: csv and json.
     
- **dumpCodes**: (all modes) If TRUE, all imported codes will be
    dumped into file `outFile`. Default: TRUE

- **outFile**: (all modes) The name of the codes dump file inside
     *`auth/` directory or a full path to it. The variable is only
     *used, if `dumpCodes` is TRUE. Available formats: csv and json.


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

    // Name of the endpoint for socket.io player connections
    playerServer: 'mygame',

    // Name of the endpoint for the socket.io admin connections
    adminServer: 'mygame/admin',

    // All options below are shared by player and admin servers.

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

For a full guide about how to create views see the [Views](Views-v2)
page.

### levels/ folder

Game levels allow the division of the experiment in multiple parts,
each of which can have a separate waiting room and separate
synchronization rules. For example, an experiment could start with a
first part where participants do not interact with each other, but
they only answer some survey questions. Only in part 2, they would
reach a waiting room and form groups for synchronous play. Introducing
multiple game levels has the advantage to reduce the number of
dropouts in later parts, for example where synchronous play is
happening.  

To create a new game level, create a new folder with the name of the
level -- e.g. "part2" -- inside the `levels/` directory. Then, inside
the new game level directory, add a `game/` folder, and optionally a
`waitroom/` folder, following exactly the same structure as for the
folders with the same name at the top-level of the directory.


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

* [Synchronization](Synchronization-v2)

## Go Back to 

* [Home](Home)
- [Game Basics](https://github.com/nodeGame/nodegame/wiki/Game-Basics-v2#directory_structure)
