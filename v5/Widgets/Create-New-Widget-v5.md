- status: complete
- version: 5.x
- follows from: [Widget-Steps](Widget-Steps-v5)

New widgets are created by registering a class to the widgets
collection `node.widgets`. See the example below.

```javascript
(function(node) {  // Self-executing function for encapsulation.

    // Register the widget in the widgets collection
    // (will be stored at node.widgets.widgets).
    node.widgets.register('NewWidget', NewWidget);

    // Add Meta-data.

    NewWidget.version = '0.0.1';
    NewWidget.description = 'This widget does this';

    NewWidget.texts = {
        // Texts here (more info on this later).
    };

    NewWidget.sounds = {
        // Sounds here (if any).
    };

    // Title is displayed in the header.
    NewWidget.title = 'New Widget';
    // Classname is added to the widgets.
    NewWidget.className = 'newwidget';

    // Dependencies are checked when the widget is created.
    NewWidget.dependencies = { JSUS: {} };

    // Constructor taking a configuration parameter.
    // The options object is always existing.
    function NewWidget(options) {
        // You can define widget properties here,
        // but they should get assigned a value in init.
    }

    NewWidget.prototype.init = function(options) {
       // Init widget variables, but do not create
       // HTML elements, they should be created in append.

       // Furthermore, you can add internal listeners here
       // or in the listeners method. (optional)
       this.on('destroyed', function() {
           // Do something. For example, notify another player.
       });
    }

    // Implements the Widget.append method.
    NewWidget.prototype.append = function() {
        // Widgets are Bootstrap panels. The following HTML
        // elements are available at the time when
        // the `append` method is called:
        //
        //   - panelDiv:   the outer container
        //   - headingDiv: the title container
        //   - bodyDiv:    the main container
        //   - footerDiv:  the footer container
        //
        this.button = document.createElement('button');
        this.button.onclick = function() {
          // Do something.
        };
        this.bodyDiv.appendChild(this.button);
    };

    // Implements the Widget.listeners method (optional).
    NewWidget.prototype.listeners = function() {
        // Listeners added here using `node.on`
        // are automatically removed when the widget
        // is destroyed.
        node.on.data('MyEvent', function() {
          // Do something.
        });
    };

    // Overwrites some default methods, for example
    // the highlight method (optional).
    NewWidget.prototype.highlight = function() {
        if (!this.panelDiv) return;
        this.panelDiv.style.background = 'red';
        this.highlighted = true;
        // Do not forget to emit the event.
        this.emit('highlighted');
    };

    // Overwrites some default methods, for example
    // the unhighlight method (optional).
    NewWidget.prototype.unhighlight = function() {
        if (!this.panelDiv) return;
        this.panelDiv.style.background = 'white';
        this.highlighted = false;
        // Do not forget to emit the event.
        this.emit('unhighlighted');
    };

})(node);
```

### Load the Widget File

Save your widget under `public/js/NewWidget.js` and then load it
from `public/index.htm`:

```js
<script src="js/NewWidget.js" charset="utf-8"></script>
```
(note: the script tag can be placed anywhere _after_ the nodeGame script and _before_ you actually use the widget in your game)


## Next

- [Learn about the texts object](Widgets-Language-v5)
