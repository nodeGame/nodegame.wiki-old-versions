- status: complete
- version: 4.x
- follows from: [Using the Widgets](Using-Widgets-v4)

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

    // Title is displayed in the header.
    NewWidget.title = 'New Widget';
    // Classname is added to the widgets.
    NewWidget.className = 'newwidget';

    // Dependencies are checked when the widget is created.
    NewWidget.dependencies = { JSUS: {} };
    
    // Constructor taking a configuration parameter.
    // The options object is always existing.
    function NewWidget(options) {
        // ...
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
        this.button.onclick = function() { ... };
        this.bodyDiv.appendChild(this.button);
    };
    
    // Implements the Widget.listeners method (optional).
    NewWidget.prototype.listeners = function() {
        // Listeners added here using `node.on` 
        // are automatically removed when the widget
        // is destroyed.
        node.on.data('MyEvent', function() { ... });
    };
    
    // Implements the Widget.destroy method (optional).
    NewWidget.prototype.destroy = function() {
        alert('I am being destroyeeed.');
    };
    
})(node);
```

Upon instantiating a new widget with `node.widgets.get` or
`node.widgets.append`, all default methods which have not been
implemented by the developer are added the instance. See the
[Widget](https://github.com/nodeGame/nodegame-widgets/blob/master/lib/Widget.js)
class for a complete reference.

## Next

- [Go back to the widgets list](Widgets-v4)

## Additional Resources

- [Inline documentation](http://nodegame.github.io/nodegame-widgets/docs/index.js.html)
- [Widgets class (node.widgets)](https://github.com/nodeGame/nodegame-widgets/blob/master/lib/Widgets.js)
- [Widget class](https://github.com/nodeGame/nodegame-widgets/blob/master/lib/Widget.js)
- [Widgets folder](https://github.com/nodeGame/nodegame-widgets/tree/master/widgets)
