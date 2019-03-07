- status: requires updates
- version: 0.6.x - 2.x

## Introduction

Widgets are interface elements, contained in a panel, and available
when nodegame-client runs in a browser. They can be re-used in
various parts of the user interface.

## Available Widgets

nodeGame comes with a set of pre-built widgets, including a timer, a round
monitor, a chat widget and many more.

- [Visual Timer](VisualTimer-Widget): displays a countdown in each
  game-step, offereing different visualization options.
  
- [Visual Round](VisualRound-Widget): displays a the count of the rounds,
  stages, and steps of the game, offering different visualization options.

- [Language Selector](LanguageSelector-Widget): builds a language
  selector form. The result of the selection are saved in `node.player.lang`.
  
- [Requirements](Requirements-Widget): executes a number of standard,
  and user-defined tests on the browser, and displays the results.

- [Chat](Chat-Widget): builds a chat interface, offering different
  communication options (one-to-many, one-to-one, etc.).
  
- [Chernoff Faces](ChernoffFaces-Widget): builds a interface to create
  configurable Chernoff faces. Also available in a simplified version.

- [Feedback](ChernoffFaces-Widget): builds a configurable feedback form.

- [Visual State](VisualState-Widget): displays a box with standard
  information such as client-id and socket-id.
  
- [Wall](Wall-Widget): displays a short log of all exchanged messages.

- [Money Talks](MoneyTalks-Widget): displays a box to visualize
  monetary earnings during a game.
  
- [D3](D3-Widget): integrates the charting and network library
  [D3](http://d3js.org/). Experimental.

And more...

## Using Widgets

Loading a widget from a nodeGame game is very easy:

```js
var root = W.getElementById('myRoot');
var timer = node.widgets.append('VisualTimer', root);

// or

var options = {};
var customWidget = new MyCustomWidget(options);
node.widgets.append(customWidget, root);
```

See the documentation of [Widgets.append](
http://nodegame.github.io/nodegame-widgets/docs/lib/Widgets.js.html#widgets.append
) for a detailed description.

## Creating Widgets

New widgets should follow the structure of
[Widget](http://nodegame.github.io/nodegame-widgets/docs/lib/Widget.js.html).
When a new widget is registered, missing methods are copied over from this base
class.

On calling `Widgets.append`, a panel is created for the widget object, which can
contain a heading, a body and a footer.
These are stored in the widget object `w` as `w.panelDiv`, `w.headingDiv`,
`w.bodyDiv` and `w.footerDiv` respectively.
The body is created automatically and so is the heading/footer as long as the
widget class has a `title`/`footer` field.

The widget's `append` method should pack all necessary HTML elements into its
`bodyDiv`.

See the [StateBar](
https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/StateBar.js)
widget for an example of a minimal yet useful example:

```javascript
(function(node) {  // self-executing function for encapsulation
    ...
    // Register the widget to be findable by Widgets.append.
    node.widgets.register('StateBar', StateBar);

    // Widget description.
    StateBar.version = '0.4.0';
    StateBar.description =
        'Provides a simple interface to change the stage of a game.';

    // This title will be placed in the heading by default.
    StateBar.title = 'Change GameStage';
    // The widget's panel will receive this class.
    StateBar.className = 'statebar';

    function StateBar(options) { ... }

    StateBar.prototype.append = function() {
        ...
        // Add elements to the bodyDiv.
        this.bodyDiv.appendChild(document.createTextNode('Stage:'));
        ...
    };
})(node);
```

## Building the Module

You can create a custom build using the make.js script in the bin directory:

```shell
# Full build.
node bin/make.js build -a

# Select which widgets to include.
node bin/make.js build -w msgbar,visualstate -o mywidgets.js

# See output for advanced usage:
node bin/make.js -h
```
