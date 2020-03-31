- status: complete
- version: 4.x
- follows from: [Using Widgets](Using-Widgets-v4)

Widgets-steps are a particular type of
[steps](Step-Callback-Functions-v4) where a widget is loaded in the
frame and waits for some input from the user. When `node.done` is
called, the widget checks if the input is correct, and in case it is
not, it will not let the player step forward. Correct and incorrect
values are stored and sent to the server when the game advances.

To create a widget-step, just extend an existing step with a `widget`
property containing the name of the desired widget,
e.g. [MoodGauge](MoodGauge-Widget-v4):

```javascript
stager.extendStep("mood", { widget: "MoodGauge" });
```

By default, the widget is loaded into an empty frame, and its current values
are checked to be correct when DONE is invoked. However, this behavior can
be changed, specifing custom options to the widget (or to the step) in the
following way:

```javascript
stager.extendStep("mood", {

    widget: {
        // Name of the widget to load.
        name: 'MoodGauge',

        // Optional custom id.
        id: 'my_widget_id', // Default: 'ng_step_widget_' + name

        // Optional id of the root element for the widget.
        root: 'append-to-this-root', // Default: an element with id
                                     // 'widget-container', or the
                                     // document.body element of the IFRAME, or
                                     // the document.body element of the page.

        // Do not append the widget,
        // just make it available in node.widgets.instances
        append: false, // Default: TRUE.

        // Do not check if values are correct (as returned by `getValues()`).
        checkValues: false, // Default: TRUE.

        // Widget-specific options.
        options: {
            choices: [ 1, 2, 3 ],
            emotions: [ 'Upset', 'Active' ]
        }
    },

    // Optional step properties.

    // Frame.
    frame: 'my/custom/frame.html', // Default: a blank page.

    // Callback.
    cb: function() { ... },        // It will be executed after the widget
                                   // is appended.

    // Done.
    done: function() { ... }      // It will be executed after the widget done
                                  // callback, and only if the return value is
                                  // not FALSE, e.g. when `checkValues()` fails.
});
```

## Next

- [Learn how to create a new widget](Create-New-Widget-v4)
