- status: complete
- version: 2.x

## Server Conf directory

Inside the installation directory of nodeGame you can find the default
configuration directory: `conf`. Inside the conf directory the
following files are available:

- servernode.js
- loggers.js
- http.js
- sio.js


### servernode.js

This file configures the behavior of the nodeGame server, setting
variables like the port, the games directory, the log directory, and
the default settings for each channel.

The file must export a function taking as input parameter the current
instance of the nodeGame Server (ServerNode class).

See the inline comments inside the default
[servernode.js](https://github.com/nodeGame/nodegame-server/blob/master/conf/servernode.js)
configuration file for available options.

### loggers.js

This file configures how much information is logged to console and to
file system. The file must export a function taking as first input
parameter the
[winston logger object](https://github.com/flatiron/winston/blob/master/README.md),
and as second parameter the path to the server log directory.

See the default
[loggers.js](https://github.com/nodeGame/nodegame-server/blob/master/conf/loggers.js)
configuration file for a working example.

### http.js

This file configures the basic routes of the Express HTTP server
inside the nodeGame server.

The file must export a function taking as first input parameter the
[Express app](http://expressjs.com/en/guide/routing.html) and as second parameter
a reference to the current instance of the nodeGame server object.
        
See the default
[http.js](https://github.com/nodeGame/nodegame-server/blob/master/conf/http.js)
configuration file for a working example.

### sio.js

This file configures the behavior of the Socket.IO server inside the
nodeGame server.
    
The file must export a function taking as first input parameter the
[Socket.io server](https://github.com/LearnBoost/Socket.IO/wiki/Configuring-Socket.IO)
and as second parameter a reference to the current instance of the
nodeGame server object.
    
See the default
[sio.js](https://github.com/nodeGame/nodegame-server/blob/master/conf/sio.js)
configuration file for a working example.


## node forever

If you want to make sure that your server stays always up you can read this great tutorial:

- http://blog.nodejitsu.com/keep-a-nodejs-server-up-with-forever
