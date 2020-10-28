- status: complete
- version: 5.x

## Introduction

The _Window API_ is nodeGame's cross-browser interface to the
browser's window.

The API is accessible through the Window object, namely `node.window`
or its shortcut `W`. It is available only in the browser environment
and it allows the game developer to load and cache pages, to
manipulate the DOM (Document Object Model), and to setup the user
interface. For example, it can do things like: disabling HTML inputs,
disabling the right click of the mouse, locking the browser's window
completely, and much more.

This guide will briefly review the main features of the API:

 - [Loading and manipulating header and frame.](#frames)
 - [Searching and Manipulating the DOM](#dom)
 - [Manipulating the User Interface](#ui)
 - [Caching frames and other static resources](#cache)

For a full description and other features refer to the
[inline documentation](
http://nodegame.github.io/nodegame-window/docs/lib/GameWindow.js.html).

<a name="frames"></a>
### Loading and manipulating header and frame.

Frames are HTML pages containing the interface of a game step. Unless
a widget is used, frames must be generally implemented by the game
developer in one ore more separate files: as static resources (in
`public/`), or as dynamic templates (in `views/`). Frames are loaded
into a dedicated
[iframe](http://en.wikipedia.org/wiki/HTML_element#Frames) which is
separate from the rest of the page.

The header is another special region of the page that can be
appositely created to contain elements that need to persist throughout
the whole game, e.g. a timer, or a counter with the accumulated
earnings. The position of the header relative to the frame can be
easily manipulated via API.

#### API methods

- **W.generateFrame()**: generates the frame.
- **W.generateHeader()**: generates the header.
- **W.generateInfoPanel()**: generates the [Info Panel](InfoPanel-API-v5).
- **W.setHeaderPosition(position)**: Moves the header in one
  of the four cardinal positions: 'top', 'bottom', 'left', 'right.'
- **W.loadFrame(uri, cb, options)**: loads a page from the given uri,
  and then executes the callback cb. This function is automatically
  called whenever the `frame` property of a step is set. The `options`
  argument can be used to alter how frames are loaded and cached the
  displayed frames. Available options: 'loadMode' and 'storeMode'.
    - _loadMode_: specifies whether the frame should be reloaded
    regardless of caching (`loadMode: 'reload'`) or whether the
    frame should be looked up in the cache (`loadMode: 'cache'`,
    default). If the frame is not in the cache, it is always
    loaded from the server.
    - _storeMode_: specifies when a loaded frame should be stored for
    caching. By default the cache isn't updated (`storeMode:
    'off'`). The other options are to cache the frame right after it
    has been loaded (`storeMode: 'onLoad'`) and to cache it when it is
    closed, that is (`storeMode: 'onClose'`). This last mode preserves
    all the changes done while the frame was open.
- **W.setUriPrefix(prefix)**: adds a prefix to the uris loaded with
  `W.loadFrame`.
- **W.adjustFrameHeight(minHeight, delay)**: Resets the min-height style
    of the iframe to fit its content properly. Normally, this method
    does not need to be called by the user, unless the length of the
    iframe is changed significantly after it has been loaded.
- **W.adjustHeaderOffset(force)**: Slides frame and/or infoPanel so that
    the header does not overlap. Normally, this method does not need to
    be called by the user.
  
<a name="dom"></a>
### Searching and Manipulating the DOM

Elements of the frame might not be immediately accessible from the
parent page. Therefore, the `W` object offers a number of methods to
access and manipulates them easily.

#### API methods

- **W.getElementById(id)**: Looks up an element with the specified id
    in the frame of the page.
- **W.setInnerHTML(search, replace, mod)**: Sets the content of
    element/s with matching id or class name or both.
- **W.searchReplace(elements, mod, prefix)**: Replaces the content of
    the element/s with matching id or class name or both.
- **W.write/writeln(text, root)**: Appends a text to the root element.
- **W.sprintf(string, args, root)**: Appends a formatted string to the
    root element.
- **W.hide/show(idOrObj)**: Hides/shows an HTML element; this method calls
  W.adjustHeaderOffset to make sure things are displayed correctly on page,
  therefore it should be preferred to manually edit the display style property
  of an element.
- **W.cssRule**: Adds a quick CSS rule to the current frame. Example:
  ```javascript
      var clear = true;
      W.cssRule('.choicetable-maintext { padding-bottom: 20px; }', clear);
  ```
  This method adds a link tag to the frame or window and stores a reference into
  `W.styleElement`. Multiple calls will adds rules, unless the clear parameter
  is set to true (default FALSE). The tag is removed and all rules are cleared
  whenever the frame is changed, but it will stay valid if the frame is shared
  across steps or stages. 

<a name="ui"></a>
### Manipulating the User Interface

The `W` object also exposes methods to directly restrict or
control some types of user action, such as: disabling the back button,
detecting when a user switches tab, displaying an animation in the
tab's header, etc.

#### API methods

- **W.disableRightClick()**: disable the right click menu.
- **W.enableRightClick()**: enables the right click menu.
- **W.lockScreen(text)**: disables all inputs, overlays a
  gray layer above the elements of the page and optionally writes a
  text on top of it.
- **W.unlockScreen()**: undoes `W.lockScreen`.
- **W.promptOnleave(text)**: displays a message if the user
  attempt to close the tab of the experiment.
- **W.restoreOnleave()**: undoes `W.promptOnleave`
- **W.noEscape()**: disables Esc terminating the connection in some
  browsers.
- **W.restoreEscape()**: undoes `W.noEscape`.
- **W.disableBackButton(disable)**: disables the back button
  if parameter `disable` is true, re-enables it if false.
- **W.onFocusIn(cb)**: executes a callback each time the
  players switches tab.
- **W.onFocusOut(cb)**: executes a callback each time the
  players returns to the experiment's tab.
- **W.blinkTitle()**: displays a rotating text on the tab of
  the experiment.
- **W.playSound(sound)**: Plays a sound, if supported by browser.

#### Configuration options

Some methods can be executed via a configuration option:

- **promptOnleave**: (true/false) see method.
- **noEscape**: (true/false) see method.
- **disableRightClick**: (true/false) see method.
- **disableBackButton**: (true/false) see method.

In addition, the following options are available: 

- **uriPrefix**: (string) sets a prefix that will be appended to all
  loaded frames (useful for multilanguage support).
- **waitScreen**: (true/false) enables/disables the grey waiting
  screen.
- **adjustFrameHeight**: (true/false) enables/disables the automatic
  adjusting of the frame's height; if false, it will be set to
  '100vh'.
- **defaultHeaderPosition**: ('top'/'bottom'/'left'/'right') sets the
  default header position.

All configuration options can be specified before the game starts in
the setup file (see the Ultimatum game
[example](https://github.com/nodeGame/ultimatum-game/blob/master/game/game.setup.js)),
or directly invoking the `init` method any time during the game, for
instance:

```javascript
W.init({ adjustFrameHeight: false });
```

<a name="cache"></a>
### Caching frames and other static resources 

Frames and other static resources can be explicitly cached at any time
during an experiment. Caching provides a twofold advantage. Firstly,
it decreases the load on the server (specially stages are repeated for
many rounds). Secondly, it guarantees a smoother game experience, by
bringing down the transition-time between two subsequent
steps. However, caching might not be available in older browser
(e.g. IE8), therefore a pre-caching test is performed automatically
upon loading nodeGame, and the results are saved in a global variable.

#### API methods
  
- **W.preCache([res1, res2, ...], cb)**: caches the requested
  resources, and executes the callback cb when finished.
- **W.cacheSupported**: flags the support for caching
  resources.
  

Caching can be enabled also via the optional `options` argument of
loadFrame method. Here it can be chosen whether a page should be
loaded from the cache if it is found there and how to cache the page
after it has been loaded. By default, the cache is read but not
updated.

#### Examples:

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


## Sources

- [Ultimatum Game Example](https://github.com/nodeGame/ultimatum/blob/master/game/client_types/includes/player.callbacks.js
)
