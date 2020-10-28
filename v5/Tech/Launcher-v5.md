- status: complete
- version: 5.x


The node launcher script comes in with a number of options as listed here:

```
  $ node launcher.js --help
Usage: launcher [options]

Options:
  -V, --version                output the version number

  ## Path to a conf file, other inline options are ignored.

  -C, --config [confFile]      Specifies a configuration file to load

  ## Inline options (more limited than a conf file).

  -c, --confDir <confDir>      Sets the configuration directory
  -l, --logDir <logDir>        Sets the log directory
  -L, --logLevel <logDir>      Sets the log level. Values: error(default)|warn|info|silly
  -g, --gamesDir <gamesDir>    Sets the games directory
  -d, --debug                  Enables the debug mode
  -i, --infoQuery              Enables getting information via query-string ?q=
  -b, --build [components]     Rebuilds the specified components
  -s, --ssl [path-to-ssl-dir]  Starts the server with SSL encryption
  -f, --default [channel]      Sets the default channel
  -P, --port [port]            Sets the port of the server (v5.12.0+)

  ## Options for Phantoms.

  -p, --phantoms <channel>     Connect phantoms to the specified channel
  -n, --nClients <n>           Sets the number of clients phantoms to connect (default: 4)
  -t, --clientType <t>         Sets the client type of connecting phantoms (default: autoplay)
  -T, --runTests               Run tests after all phantoms are game-over (overwrites settings.js in test/)
  -k, --killServer             Kill server after all phantoms are game-over
  -a, --auth [option]          Phantoms auth options. Values: new(default)|createNew|nextAvailable|next|code|id:code&pwd:password|file:path/to/file.
  -w, --wait [milliseconds]    Waits before connecting the next phantom. Default: 1000
  -h, --help                   output usage information
```

### Examples

#### Phantoms

Phantoms are headless browsers playing the game (note: you need an `autoplay` or `phantom` client type enabled in the client_types directory of your game).

- Connect 6 phantoms to the channel named 'foo'.

```javascript
node launcher -p foo -n 6
```

- Connect 6 phantoms to the channel named 'foo' where authorization is
enabled.

```javascript
node launcher -p foo -n 6 -a
```

#### Rebuild

- Rebuild the specified modules before launching the server

```javascript
node launcher -b window,widgets
```

## Go Back to

* [Home](Home)
