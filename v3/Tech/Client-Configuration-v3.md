- status: complete
- version: 3.x


Usually, there is no need to manually configure a node instance, as
all default values are chosen automatically. However, the `node.setup`
method allows one to obtain a fine-grained configuration in cases when
it is needed. The syntax is the following:

```javascript
node.setup('feature', settings);
```

## Features configurable via setup

- **"verbosity"**:
```javascript
// Sets the verbosity level for console output.
// Verbosity is an integer value, or one of the available levels:
// 
//   - 'ALWAYS': -Number.MAX_VALUE,
//   - 'error': -1,
//   - 'warn': 0,
//   - 'info': 1,
//   - 'silly': 10,
//   - 'debug': 100,
//   - 'NEVER': Number.MAX_VALUE
//
// Current value is stored in node.verbosity. Default: 'warn'.
node.setup('verbosity', 'warn');
```

- **"env"**:
```javascript
// Sets environment variables.
// Environment variables can be retrieved with node.env('myenv').
node.setup('env', { myenv: true });
```

- **"nodename"**:
```javascript
// Assigns a name to the node instance.
// The name is stored under node.nodename, and it is added to every
// log message. Default: 'ng'.
node.setup('nodename', 'mynode');
```

- **"debug"**:
```javascript
// If TRUE, errors are thrown, otherwise they are caught.
// In production, 'debug' should be set to FALSE.
node.setup('debug', true);
```

- **"events"**:
```javascript
// Configures the Event Emitter.
node.setup('events', {
    // Logs to console all fired events.
    dumpEvents: false,
    // Stores a record of all fired events.
    history: false
});
```

- **"settings"**:
```javascript
// Adds/updates variables in node.game.settings.   
node.setup('settings', {
    foo: 'bar',
    foo2: 'bar2'
});
```

- **"metadata"**:
```javascript
// Adds/updates variables in node.game.metadata.
node.setup('metadata', {
    name: 'The name of the Game',
    version: '1.0.0'
});
```

- **"player"**:
```javascript
// Setups a new player object.
// A new player cannot be created while a game is running.
node.setup('player', conf);
```

- **"timer"**:
```javascript
// Setup an existing timer object.
// Accepts one configuration parameter of the type:
//
//   - 'name': name of the timer. Default: node.game.timer.name
//   - options: configuration options to pass to the init method
//   - action: an action to call on the timer (start, stop, etc.)
//
// If options is an array, it will repeat the configuration of
// timers for each element in it.
node.setup('timer', options);
```

- **"plot"**:
```javascript
// Modifies the current list stages and steps of the game
// plot must be an object containing the updated stages,
// and updateRule the string 'replace', or 'append'.
node.setup('plot', plot, updateRule);
```

- **"plist"**:
```javascript
// Modifies the current list of connected players.
// plist must be an array of players, and updateRule
// the string 'replace', or 'append'.
node.setup('plist', plist, updateRule);
```

- **"lang"**:
```javascript
// Sets the default language.
node.setup('lang', {
    name: 'English',
    shortName: 'en',
    nativeName: 'English',
    path: 'en/'
});
// Or:
node.setup('lang', 'English'); // Uses the first two letters to make shortName.
// Or:
node.setup('lang', [ 'English', true ]); // Sets en/ prefix for loading frames.
```

- **"host"**:
```javascript
// Sets the uri of the host to connect to.
// By default, the host is retrieved from the value of window.location.
node.setup('host', 'http://myhost.com');
```

- **"socket"**:
```javascript
// Sets the type of socket connections with additional options.
node.setup('socket', {
    type: 'SocketIo',
    reconnect: false
});
```

## Features available in the browser only

### Window

- **"window"**:
```javascript
node.setup('window', {
    // Displays a confirmation message if the user tries to leave the page.
    promptOnleave: false,
    // Pressing escape does not close the connection.
    noEscape: true,
    // Default position of the header.
    defaultHeaderPosition: 'left',
    // Does not display a context menu with the right click.
    disableRightClick: false,
    // Disables the back button in the browser interface.
    disableBackButton: true,
    // Sets the uri prefix for every new frame load (see W.setUriPrefix).
    uriPrefix: 'en-us',
    // Creates and configures a new wait screen (see WaitScreen).
    waitScreen: { ... },
    // Destroys the wait screen.
    waitScreen: false    
});
```

- **"page"**:
```javascript
node.setup('page', {
   // Removes all elements in the body of the page.
   clearBody: true,
   // Removes all elements from page.
   clear: true,
   // Sets the title of the page.
   title: 'ABC',
   // Sets the title of the page and also adds a H1 block in the body.
   title: {
       title: 'ABC',
       addToBody: true
   }
});
```

- **"frame"**:
```javascript
node.setup('frame', {
   // Generates a new frame.
   generate: {
       // The id of the HTML element to which to append the frame.
       root: 'myid',
       // If TRUE, a new frame is created even if an existing one is found.
       force: true,
       // The name of the frame (default: 'ng_mainframe').
       name: 'foo',       
   },
   // Load a new frame.
   load: 'myframe.html',
   // Load a new frame with options.
   load: {
       url: 'myframe.html',
       // Callback to execute after the frame has been loaded.
       cb: function() { console.log('loaded!'); },
       // Options to pass to W.loadFrame.
       options: { cache: { loadMode: 'reload' } }
   },
   // Sets the uri prefix for every new frame load (see W.setUriPrefix).
   uriPrefix: 'en-us',
   // Clears the frame contents.
   clear: true,
   // Destroys the frame and removes it from DOM.
   destroy: true
});
```

- **"header"**:
```javascript
node.setup('header', {
   // Generates a new frame.
   generate: {
       // The id of the HTML element to which to append the header.
       root: 'myid',
       // If TRUE, a new header is created even if an existing one is found.
       force: true,
       // The name of the header (default: 'ng_header').
       name: 'foo',       
   },
   // Sets the position of the header on the page.
   position: 'top', // or 'bottom', 'left', 'right'.
   // Clears the header contents.
   clear: true,
   // Destroys the header and removes it from DOM.
   destroy: true
});
```

#### Widgets

- **"widgets"**:
```javascript
node.setup('widgets', {
   // Registers new widgets.
   widgets: {
       // Widget 1.
       mywidget: function() { ... },
       // Widget 2.
       mywidget2: function() { ... }
   },
   // Destroys all widgets.
   destroyAll: true,
   // Appends a new widget.
   append: {
      VisualRound: {
          // The id of the root element (default: append to page).
          root: 'rootId', 
          // Other options for the widget.
      }
   }
});
```

- **waitroom"**:
```javascript
// See WaitingRoom widget for all configuration options.
node.setup('waitroom', { ... } });
```

- **requirements"**:
```javascript
// See Requirements widget for all configuration options.
node.setup('requirements', { ... } });
```

## Configure them all

Alternatively, the configuration for each element can be grouped
together in a single object and call the setup method with the
'nodegame' label.

```javascript
node.setup('nodegame', {
    debug:  true,
    window: { ... },
    // ...
});
```

## Remote configuration

Every logic can remotely invoke setup functions via the following command:

```javascript
node.remoteSetup('feature', 'recipientId');
```

## Current configuration

All settings specified with `node.setup` are stored in the `node.conf` object.
