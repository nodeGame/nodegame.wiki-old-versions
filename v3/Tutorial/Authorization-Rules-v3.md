- status: complete
- version: 3.x
- follows from: [Requirements Checkings](Requirements-Checkings-v3)

Authorization rules are optional, but highly recommended in online
experiments. They are specified in the folder `auth/` of the game. The
following files are available:

* auth.settings.js
* auth.js

## File: auth.settings.js

This file exports an object containing the authorization settings. To
enable authorization for your channel set:

```javascript
enabled: true
```
    
If authorization is enabled, then the next variable examined is the
authorization `mode`:

   1. **dummy**: Creates dummy ids and passwords in sequential order
   starting from 0. Useful for debugging purposes.
   
   2. **auto**: Creates random 8-digit alphanumeric ids and passwords.
   
   3. **local**: reads the authorization codes from a file. By
                 default, `codes.json` and `code.csv` are tried in
                 sequence. A custom file can be specified using the
                 `inFile` variable (supported formats: json and csv).
                 
   4. **remote**: fetches the authorization codes from a remote URI.
                 The only supported protocol at the moment is the
                 [DeSciL](http://www.descil.ethz.ch) protocol.
                 
   5. **custom**: The `customCb` property will be executed in order to
                 get the authorization codes. The input parameters
                 are: the settings object itself, and a callback for
                 asynchronously returning the codes.

If authorization is enabled, players must connect through the address:

    http://localhost:8080/myexperiment/auth/id/password 

(assuming you are running your experiment in your local machine, and
that experiment is called _myexperiment_).

Notice that the password parameter can be omitted if it not specified
in the codes (see below).

### Codes format

A code object contains an id field and optionally a pwd field. For
example:

```javascript
{ 
    id: 'foo', 
    pwd: 'optional_password',
} 
```

Moreover, any additional property (besides `id`, and `pwd`) will be
added to the code objects stored in the channel registry.


### Authorization variables

Additional variables are used by the different authorization modes, as
explained below.

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
     
### Claiming Ids
     
Sometimes, the experimenter does not have full control on the clients
arriving to the experimental server. This is often the case in online
labor markets, such as Amazon Mechanical Turk. In these situations,
preregistering the id of incoming clients is a useful practice. To do
so enable _claimId_:
     
- **claimId**: (all modes) If TRUE, remote clients will be able to
    claim an id via GET request to your channel address +
    '/claimid'. Default: FALSE.
    
ClaimId GET request must contain at least an _id_ field (usually the
id of the worker on the online labor market). This _id_ is exchanged
with a valid nodeGame id, which is sent back to the client, and that
can be used to create an authorization link.

Other useful options in the process are:

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
    
## File: auth.js

This file allows the game developer to define a number of callbacks to
fine-tune the authorization mechanisms of your channel. The file must
export a function that accept a configuration object as input
parameter.

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


## Next

* [Synchronization](Synchronization-v3)

## Go Back to 

* [Home](Home)
