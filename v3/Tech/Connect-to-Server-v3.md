- status: complete
- version: 3.x

## Introduction

A `node` instance can connect to a nodeGame server through two types
of sockets:

- **SocketIO**: connects to the server through different network
  protocols using [Socket.IO](http://socket.io). It used, for example,
  for connections coming from a browser.
- **SocketDirect**: connects to the server via a shared object in the
  server. It is used for example by a logic, or a bot to connect to a
  game room.

### Selecting a socket and connecting

Connect via _SocketIO_:

```javascript
node.setup('socket', {
    type: 'SocketIo',
    // Other Socket.Io options, e.g:
    reconnect: false
});

node.connect('/mychannel');
```

Connect via _SocketDirect_:

```javascript
// A reference to the channel's socket must be obtained.
var channelSocketObject = channel.socket.
node.socket.setSocketType('SocketDirect', {
    socket: channelSocketObject
});

node.connect();
```

### Connect parameters

The method `node.connect` accepts an optional second parameter that
contains options to instruct the server how to handle the new
connection. For example, it is possible to specify the
[client type](Client-Types-v3) and the starting room for the
connection.

```javascript
// SocketIO options.
var ioOptions = {
    query: 'clientType=player&startingRoom=myRoom'
};
node.connect('/mychannel', directOptions);

// SocketDirect options.
var directOptions = {
    clientType: 'player',
    startingRoom: 'myRoom'
};
node.connect(null, directOptions);
```

In production, it is recommended to disable connect parameters for
SocketIo connection. To do so, modify the `sioQuery` option in the
[channel configuration file](Channel-Configuration-v3).
