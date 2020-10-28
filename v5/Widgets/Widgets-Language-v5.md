- status: complete
- version: 5.x
- follows from: [Create a New Widget](Create-New-Widget-v5)

All the text displayed to the user should be parametrized: instead of hardcoding it, make use of the `texts` object to store all your text strings.

This has the key advantage of enabling the easy replacing of any text with another, without changing the code of the widget.

### Defining Default Texts

 If you are creating a new widget, define a static `texts` variable with all the display strings:

```javascript

/** ### NewWidget.texts
 *
 * Default texts to display
 */
NewWidget.texts = {

    // Variables in here contains display text.
    welcomeMessage: 'Hello!',

    // HTML is supported.
    disconnect: '<span style="color: red">You have been ' +
        '<strong>disconnected</strong>.</span><br>',

    // Parametric text is supported.
    gameMode: function(widget, param) {        
        if (param === 'PRACTICE') {
            return 'You are in practice mode.';
        }
        return 'This is the real game.';
    },

    // More text variables can be added as needed.
};
```

### Getting Default Texts

Use the `getText` method to access the requested text when your widgets needs it. For instance:

```js
// Inside a method of the NewWidget class.

this.getText('welcomeMessage'); // 'Hello!'

this.getText('disconnect'); // '<span style="color: red">You have been ' +
                            // '<strong>disconnected</strong>.</span><br>'

this.getText('gameMode', 'PRACTICE'); // 'You are in practice mode.'
```

### Overwriting Default Texts

If you need to change the value of any default text, you can use the `setText` method or initialize the widget with a custom `texts` property. For instance, this translates your widget in Spanish:

```js
node.widget.append('NewWidget', root, {
    texts: {
        welcomeMessage: 'Hola!',
        disconnect: '<span style="color: red">Usted esta ' +
                    '<strong>desconnectado</strong>.</span><br>',
        gameMode: function(widget, param) {        
            if (param === 'PRACTICE') {
                return 'Usted esta en practice mode.';
            }
            return 'Ya empezamos!';
        }
    }
})
```

## Next

- [Go back to the widgets list](Widgets-v5)

## Additional Resources

- [Inline documentation](http://nodegame.github.io/nodegame-widgets/docs/index.js.html)
- [Widgets class (node.widgets)](https://github.com/nodeGame/nodegame-widgets/blob/master/lib/Widgets.js)
- [Widget class](https://github.com/nodeGame/nodegame-widgets/blob/master/lib/Widget.js)
- [Widgets folder](https://github.com/nodeGame/nodegame-widgets/tree/master/widgets)
