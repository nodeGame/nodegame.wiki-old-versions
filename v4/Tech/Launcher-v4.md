- status: complete
- version: 4.x


The node launcher script comes in with a number of options as listed here:

```
  node launcher.js --help

  Usage: launcher.js [options]

  Options:

    -h, --help                   output usage information
    -V, --version                output the version number
    -C, --config [confFile]      Specifies a configuration file to load
    -c, --confDir <confDir>      Sets the configuration directory
    -l, --logDir <logDir>        Sets the log directory
    -g, --gamesDir <gamesDir>    Sets the games directory
    -d, --debug                  Enables the debug mode
    -i, --infoQuery              Enables getting information via query-string ?q=
    -b, --build [components]     Rebuilds the specified components
    -s, --ssl [path-to-ssl-dir]  Starts the server with SSL encryption
    -p, --phantoms <channel>     Connect phantoms to the specified channel
    -n, --nClients <n>           Sets the number of clients phantoms to connect (default: 4)
    -t, --clientType <t>         Sets the client type of connecting phantoms (default: autoplay)
    -T, --runTests               Run tests after all phantoms have reached game over (overwrites settings.js in test/ folder
    -k, --killServer             Kill server after all phantoms have reached game over
    -a --auth [option]           Phantoms pass through /auth/. Options: createNew|new|nextAvailable|next|code|id:code&pwd:password|file:path/to/file. Default: "new".
    -w --wait [milliseconds]     Waits before connecting the next phantom. Default: 1000
```

### Examples

Connect 6 phantoms to the channel named 'foo'.

```javascript
node launcher -p foo -n 6
```

Connect 6 phantoms to the channel named 'foo' where authorization is
enabled.

```javascript
node launcher -p foo -n 6 -a
```

Rebuild the specified modules before launching the server

```javascript
node launcher -b window,widgets
```

## Go Back to 

* [Home](Home)
