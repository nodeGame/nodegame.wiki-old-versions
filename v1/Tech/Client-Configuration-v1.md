- status: complete
- version: 1.x

## The Setup Method

The `node.setup` method allows a fine-grained configuration of a `node`
instance.

```javascript
// Address of the host to connect to.
node.setup('host', 'http://myhost.com');

// Type of socket connections with additional options.
node.setup('socket', {
    type: 'SocketIo',
    reconnect: false
});

// Verbosity level.
node.setup('verbosity', 'warn');

// Global environment variables.
node.setup('env', { myenv: true });

// Assigns a name to the node instance.
node.setup('nodename', 'mynode');

// If TRUE, throws all errors.
node.setup('debug', true);

// Event Emitter settings.
node.setup('events', {

    // Logs to console all fired events.
    dumpEvents: false,

    // Stores a record of all fired events.
    history: false
});

// Game Settings.
node.setup('game_settings', {

    // How many update of stage / stageLevel, etc. to publish.
    publishLevel: 0,

    // Will send a start / step command to the other clients
    // as this clients will step through the game stages.
    syncStepping: true
});

// Adds game metadata.
node.setup('game_metadata', {
    name: 'The name of the Game',
    version: '1.0.0'
});

// Setups a new player object.
// A new player cannot be created when a game is running.
node.setup('player', conf);

// Operates an existing timer with the specified name.
// options must contain the property action:
// 'start', 'stop', 'restart', 'pause', or resume.
node.setup('timer', name, options);

// Modifies the current list stages of the game
// plot must be an object containing the updated stages,
// and updateRule the string 'replace', or 'append'.
node.setup('plot', plot, updateRule);

// Modifies the current list of connected players.
// plist must be an array of players, and updateRule
// the string 'replace', or 'append'.
node.setup('plist', plist, updateRule);

// Sets the default language
node.setup('lang', {
    name: 'English',
    shortName: 'en',
    nativeName: 'English',
    path: 'en/'
});

// Only available in the browser.

node.setup('window', {

    // Displays a confirmation message
    // if the user tries to leave the page.
    promptOnleave: false,

    // Pressing escape does not close the connection.
    noEscape: true,

    // Default position of the header.
    defaultHeaderPosition: 'left',

    // Does not display a context menu
    // with the right click.
    disableRightClick: false
});
```

Alternatively, the configuration for each element can be grouped together in a
single object and call the setup method with the 'nodegame' label.

```javascript
node.setup('nodegame', {
    debug:  true,
    window: { ... },
    env:    { ... }
});
```

All settings specified with `node.setup` are stored in the `node.conf` object.
