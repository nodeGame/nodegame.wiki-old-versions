- status: fairly complete
- version: 2.x

## Introduction

`nodegame-window` provides a API to interface nodeGame with the
browser window.

It exposes method to creates a page header and a main iframe document,
where html pages, CSS and javascript libraries, can be easily loaded
and unloaded. 

All loaded resources can be cached to guarantee a smooth and fast
transition between thee different steps of a game.

It also exposes methods that can limit user's interaction with the
page, such as: disabling HTML input, blocking the right click of the
mouse, or even completely locking the browser's screen (useful when,
for example, when the game is paused).

It automatically takes care of most cross-browsers incompatibilities.

## Usage

Refer to the documentation of [GameWindow](
http://nodegame.github.io/nodegame-window/docs/lib/GameWindow.js.html) and its
modules for a full description.
The methods of `GameWindow` can be accessed over the global `W` object.

Additionally, the client code of the Ultimatum game has some well-documented
usage of the `GameWindow` [here](
https://github.com/nodeGame/ultimatum/blob/master/game/client_types/player.js) and
[here](
https://github.com/nodeGame/ultimatum/blob/master/game/client_types/includes/player.callbacks.js
).

### Frame loading

The main contents of a page are normally contained within an iFrame.
This frame has to be created first if it doesn't exist yet:

```javascript
if (!W.getFrame()) W.generateFrame();
```

After this, pages can be loaded from URLs using the `loadFrame` method.
A callback can be given as an optional parameter which will be called once the
page is fully loaded.

```javascript
W.loadFrame('/mygame/intro.html', function() {
    W.getElementById('label').innerHTML = 'Loaded!';
});
```

There is the possibility to specify JavaScript files to be inserted whenever
frames are loaded.
Scripts can be loaded for all pages or just specific pages.
This is achieved with `initLibs`, which has to be called before using
`loadFrame`:

```javascript
// Adds 'util.js' to all pages, 'intro-fun.js' just to 'intro.html'.
W.initLibs(['/mygame/util.js'],
    {'/mygame/intro.html': '/mygame/intro-fun.js'});
```

### Screen locking

The entire page can be grayed out and all inputs disabled using `lockScreen`.
An optional parameter specifies a message to be shown over the screen.
`unlockScreen` undoes the lock and the state of the lock can be queried with
`isScreenLocked`.

Example:

```javascript
W.lockScreen('You must wait.');
setTimeout(function() {
    if (W.isScreenLocked()) W.unlockScreen();
}, 3000);
```

### Header manipulation

The header is an area separate from the main iFrame, useful for showing
information that should persist over several frame loads.

After generating the header, elements can be added to it and its position
relative to the central frame can be changed:

```javascript
var header = W.getHeader() || W.generateHeader();
W.setHeaderPosition('bottom');
header.appendChild(document.createTextNode('You are using a computer.'));
```

### Caching

**NOTE**: Caching is still an experimental feature.

In the case that some pages are re-loaded frequently in the course of one game,
`GameWindow` provides utilities to cache their contents once on the client side
and load them from memory instead of requesting them from the server each time.

Controlling the caching is mainly done via the optional `options` argument of
[loadFrame](
http://nodegame.github.io/nodegame-window/docs/lib/GameWindow.js.html#gamewindow.loadframe
).
Here it can be chosen whether a page should be loaded from the cache if it is
found there and how to cache the page after it has been loaded.
By default, the cache is read but not updated.

If it is known in advance which pages will be used frequently, one can pre-load
them before the game is started using [preCache](
http://nodegame.github.io/nodegame-window/docs/lib/GameWindow.js.html#gamewindow.precache
).

Examples:

```javascript
// Pre-load some pages.
W.preCache(['page1.html', 'page2.html'], function() {
    startMyGame();
});

// Add a new page to the cache.
W.loadFrame('page3.html', undefined, {
    storeMode: 'onLoad'
});

// Force the download of a cached page from the server.
W.loadFrame('page1.html', undefined, {
    loadMode: 'reload'
});
```

## Building the Module

You can create a custom build using the make.js script in the bin directory:

```shell
# Full build, about 20 KB minified.
node bin/make.js build -a

# Bare build, about 8 KB minified.
node bin/make.js build -B

# See output for advanced usage:
node bin/make.js -h
```
