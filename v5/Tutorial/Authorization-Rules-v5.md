- status: complete
- version: 5.x
- follows from: [Requirements Checkings](Requirements-Checkings-v5)

Authorization rules are optional, but highly recommended for online
experiments. Rules are defined in the folder `auth/`, in files:

* _auth.settings.js_: contains the main authorization settings;
* _auth.js_: contains extra settings for advanced setups.

## File: auth.settings.js

To enable authorization for your channel set:

```javascript
enabled: true
```

Then, specify the authorization `mode`:

   1. **dummy**: creates dummy ids and passwords in sequential order
   starting from 0. Useful for debugging purposes.

   2. **auto**: creates random 8-digit alphanumeric ids and passwords.

   3. **local**: reads the authorization codes from a file. By
                 default, `codes.json`, `codes.js`, and `code.csv` are
                 tried sequentially. A custom file can be specified
                 using the `inFile` variable (supported formats: json,
                 js, and csv).

   4. **remote**: fetches the authorization codes from a remote URI.
                 The only supported protocol at the moment is the
                 [DeSciL](http://www.descil.ethz.ch) protocol. **This
                 option is temporarily disabled**

   5. **custom**: the `customCb` property will be executed in order to
                 get the authorization codes. The input parameters
                 are: the settings object itself, and a callback for
                 asynchronously returning the codes.

### Authorization Settings

- **nCodes**: (modes: 'dummy', 'auto') The number of codes to
    create. For online experiments, it is recommended to create 3 to 5
    more codes than assignments. Default: 100.

- **addPwd**: (modes: 'dummy', 'auto') If TRUE, a password field is
    added to each code. Default: FALSE.

- **codesLength**: (modes: 'auto') The length of generated
    codes. Default: `{ id: 8, pwd: 8, AccessCode: 6, ExitCode: 6 }`

- **inFile**: (modes: 'local') The name of the codes file inside `auth/`
   directory or a full path to it. Supported formats: csv and json.

- **dumpCodes**: (all modes) If TRUE, all imported codes will be
    dumped into file `outFile`. Default: TRUE.

- **outFile**: (all modes) The name of the codes dump file inside
     `auth/` directory or a full path to it. The variable is only used
     if `dumpCodes` is TRUE. Available formats: csv and json.

- **customCb**: (modes: 'custom') The custom callback to create
  authorization codes associated with mode 'custom'.

### Linking Ids from Third-Party Services ("Claim Id")

If your participants are recruited through third-party services, such
as Amazon Mechanical Turk, you need to link the id of the user on the
external platform with the id on nodeGame. To do so enable _claimId_:

- **claimId**: (all modes) If TRUE, remote clients can claim a
    nodeGame id with a GET request to your channel address +
    `/claimid`. Default: FALSE.

ClaimId GET request must contain at least an _id_ field (usually the
id of the worker on third-party service). This _id_ is exchanged
with a valid nodeGame id, which is sent back to the client, and that
must be used to create a valid authorization link. You can check the
[mturklinkpage.html](https://github.com/nodeGame/nodegame-mturk/blob/master/public/mturklinkpage.html)
from the [nodeGame-mturk
package](https://github.com/nodeGame/nodegame-mturk) package for an
example.

Other Claim Id options are:

- **claimIdValidateRequest**: (all modes) Must return TRUE if a
    requester is authorized to claim an id.
     ```javascript
    function(query, headers) {
        // Check query and headers...
        return true;
    }
    ```

- **claimIdPostProcess**: (all modes) Manipulates the client object
    after the claim id process succeeded. E.g.:
    ```javascript
    function(code, query, headers) {
        code.WorkerId = query.id;
    }
    ```

- **claimIdModifyReply**: manipulates the object sent back to the client.
    ```javascript
    function(reply, clientObj) {
        // If claimId and Game server are different, notify the client
        // about the address of the game server.
        if (reply.code) reply.host = 'http://mygameserver.com';
    }
    ```

### Game Address with Authorization On

If authorization is enabled, players and monitor must connect through
a special address. Assuming your experiment runs on your local machine
and it is called _myexperiment_, the address for the players is:

    http://localhost:8080/myexperiment/monitor/auth/id/password

(id and password is loaded/created by the `mode` parameter; password
can be omitted, see "Codes format" below).

The monitor address is:

    http://localhost:8080/myexperiment/auth/id/password


(where id and password are as specified in file
[channel.credentials.js](https://github.com/nodeGame/nodegame/wiki/Channel-Configuration-v5#file-channelcredentialsjs)).


### Codes format

A code object contains an id field and optionally a pwd field. For
example:

```javascript
{
    id: 'foo',
    pwd: 'optional_password' // Can be omitted.
}
```

Moreover, any additional property (besides `id`, and `pwd`) will be
added to the code objects stored in the channel registry.

## File: auth.js

This file allows the game developer to define a number of callbacks to
fine-tune the authorization mechanisms of your channel. Usually, there
is no need to modify (or create) this file.

The file must export a function that takes as input parameter an object that exposes three methods: `authorization`, `clientIdGenerator` and `clientObjDecorator`.

Each of these methods accepts 1 or 2 parameters; the last one must be
a callback function and the first one is the name of the server
('player' or 'admin') to which the callback should be applied to. If
only the callback is provided, then it is applied to both 'player' and
'admin' servers.


* **authorization**: This function must return true to authorize the
    connection. It accepts as input parameters: a reference to the
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

* **clientIdGenerator**: This function must return a string
    representing the id for the connecting client, or undefined to
    fallback to the default id-assignment routine. It takes the same
    input parameters as the authorization function.


* **clientObjDecorator**: This function does not have a return value, but it  
    simply modifies the client object. It takes  two parameters as
    input: the first one is current client object, and second one is
    the info object, as described in the authorization function. The
    properties of the client object depend on the server
    configuration. Modification to the client object will be
    immediately effective, however, some properties can never be
    modified, or an error will be thrown: `id`, `sid`, `admin`, `clientType`.

### Example

```js
module.exports = function(auth) {

  // The first parameter 'player' indicates that these authorization settings   // apply only to players (i.e., no logics).
  auth.authorization('player', function(channel, info) {
    // TRUE, means client is authorized.
    return true;
  });

  // Applies to players only.
  auth.clientIdGenerator('player', function(channel, info) {
    // Returns a valid client ID (string) or undefined to default to
    // assign an id automatically.
    return;
  });

  // This applies to all clients (players and logics)
  auth.clientObjDecorator(function(clientObj, info) {
      if (info.headers) clientObj.userAgent = info.headers['user-agent'];
  });
```

## Next

* [Levels](Levels-v5)

## Go Back to

* [Home](Home)
