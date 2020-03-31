- status: complete
- version: 4.x
- follows from: [Views](Views-v4)
    

### Client-Side Logging

Client-side logging is controlled by the variable `node.verbosity`. 

In general, the higher its value, the more information will be printed
to the JavaScript console of the browser -- or to the standard output,
if the client is executed as a bot or as logic on the server.

The following log levels are pre-defined:

- **ALWAYS**: -Number.MAX_VALUE,
- **error**: -1,
- **warn**: 0,
- **info**: 1,
- **silly**: 10,
- **debug**: 100,
- **NEVER**: Number.MAX_VALUE

By default, `node.verbosity` is set to 0, so that only warnings and
errors are logged. Its value can be set directly, or through the setup
function (which accepts also strings).

```js
node.verbosity; // 0
node.verbosity = 100; // "debug"
node.setup('verbosity', 'debug');
node.setup('verbosity', 100);
```

#### Custom logging

To create custom logging for your game, you can make use of the
function:

```js
node.log(text, level, prefix)
```

where `level` is one of the pre-defined levels (default 'info'), and
`prefix` is an optional text to display before the log message.

The default log message includes the name of the node instance,
`node.nodename` (default: `ng`) and the current game stage.

```js
node.log('Player did something strange.');
// ng@1.2.1 - 18:50:53:452 > Player did something strange. 
```

Shortcuts are available to log text only if `node.verbosity` is
greater or equal to the corresponding level:

- **node.err(txt)** 
- **node.warn(txt)** 
- **node.info(txt)** 
- **node.silly(txt)**

```js
node.warn('Player did something strange AGAIN.');
// ng@0.0.0 - 18:53:12:631 > warn - Player did something strange AGAIN.
```

#### Remote logging

The variable `node.remoteVerbosity` controls what log messages are
also sent to server. The same verbosity levels for local logging apply
for remote verbosity, and by default only errors are sent to the
server.

```js
node.remoteVerbosity; // -1;
node.warn('Player did something strange AGAIN.'); // Log locally only.
node.err('Player is cheating.'); // Log locally and remotely.
```
Depending on the server's setting, the message can be saved to file
system, database, other storage, or ignored. If saved, it is formatted
as follows:

```js
{
    "name": "ultimatum",
    "level":"info",
    "message": "{\"id\":751791,\"session\":\"8114736763875356\",\"stage\":{\"stage\":1,\"step\":2,\"round\":1},\"action\":\"say\",\"target\":\"LOG\",\"from\":\"53049620184807\",\"to\":\"SERVER\",\"text\":\"error\",\"data\":\"FUNCK\",\"priority\":0,\"reliable\":1,\"created\":\"2017-03-19T23:01:42.624Z\",\"forward\":0}","timestamp":"2017-03-19T23:01:42.631Z"}
}
```

To visualize client errors on the server console, you can define an
event listener on 'in.say.LOG':

```js
// Log errors from remote clients to server's console,
// happening after the game has started (stage > 0).
node.on('in.say.LOG', function(msg) {
    if (msg.text === 'error' && msg.stage.stage) {
        console.log('Error from client: ', msg.from);
        console.log('Error msg: ', msg.data);
    }
});
```


### Server-Side Logging

Server side logging is based on
[Winston.js](https://github.com/winstonjs/winston/blob/master/README.md),
and can be controlled through the configuration file: `loggers.js`. 

By default, there are 4 configuration files:

- **servernode.log**: contains info about loading the nodegame-server
    and its games, and all errors that are not caught elsewhere at run-time.

- **channel.log**: contains info and errors about loading channels,
    players moved within a channel, messages that failed delivery.

- **messages.log**: contains all messages that have been exchanged
    with the server, including those sent by bots and logics

- **clients.log**: contains all console output of bots and phantoms
    executed on the server.
    
You can know more about Server-side configuration
[here](Server-Configuration-v4).    

## Sources

- [Log Module for Client](https://github.com/nodeGame/nodegame-client/blob/master/lib/modules/log.js)
- [Log Configuration File for Server](https://github.com/nodeGame/nodegame-server/blob/master/conf/loggers.js)
- [Winston Logging README](https://github.com/winstonjs/winston/blob/master/README.md)

## Next

Next: [Configure nodeGame for live experiments](Go-Live-v4)

## Go Back to 

* [Home](Home)
