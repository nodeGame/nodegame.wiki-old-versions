- status: complete
- version: 5.x
- follows from: [Client Types](Client-Types-v5)

## Default Step Properties

Game developers can define their step properties (as many as needed), but some names are reserved by the nodeGame engine for special purposes. The most important reserved properties are:

 * **frame**: url of the page to load, or an object containing options, or a callback returning an object or an url.
 * **cb**: the callback (cb) function to be executed after the frame
 is loaded, but before the player can interact with it.
 * **done**: callback function executed after `node.done` (example below).
 * **timer**: duration of current step (in milliseconds).
 * **timeup**: callback function executed when the timer expires.
 * **init**, **exit**: callback functions executed at the beginning and end of a step or stage (example below).

 #### Done callback

The done callback receives the same input parameters of `node.done` and can modify them or completely halt the procedure to terminate the current step if some conditions are not satisfied.

 ```javascript
 stager.extendStep('instructions', {
     cb: function() {
        // In this example, we just call node.done after 2 seconds.
        setTimeout(function() {
            // The value passed to node.done is evaluated by the done callback.
            node.done({ foo: 'bar' });
        }, 2000);
     },
     // The done callback evaluates if conditions for terminating the
     // current step are satisfied and/or modifies done parameters.
     done: function(results) {
         // results is equal to { foo: 'bar'}.         
         // If some conditions are not satisfied, the done callback can
         // block the execution by returning false.
         if (results.foo !== 'bar') return false;
         // The done callback can also modify the results that will sent
         // to the server.
         results.foo = 'bar2';
         return results;
         // Notice: for retro-compatibility returning true is ignored and
         // not sent to the server. If you need to send a truthy value,
         // consider sending 1, or [true].
     }
 });
 ```

### Frame property validity.

By default, a frame is reloaded only between consecutive stages, or consecutive
rounds. This means that within the same stage, subsequent steps will continue
using the same frame loaded at step 1 of the stage.

### Init and Exit callbacks

It is possible to execute callbacks just before and after a certain
step is executed. Just add the `init` and `exit` properties to the
step object. For example:

```javascript
stager.extendStep('instructions', {
    init: function() {
        // See the next section of the tutorial
        // to know more about the `node.on` command.
        node.on('myEvent', function() {
             console.log('I am valid only during this step');
        });
        console.log('I am executed before the step callback');
    },
    exit: function() {
        console.log('I come after the step is done, before a new ones begins.');
    }
});
```

Likewise, it is possible to add `init` and `exit` callbacks to the
stage object as well.

```javascript
// Notice extend**Stage**.
stager.extendStage('game', {
    init: function() {
        node.on('myEventInGame', function() {
             console.log('I am valid through all the steps and all the ' +
                         'rounds of the stage game.');
        });
        console.log('I am executed before any step in the game stage.');
    },
    exit: function() {
        console.log('I come after the stage is done, before a new one begins.');
    }
});
```

**Important!** The `init` and `exit` properties behave differently than other properties: they are **not** inherited by each step inside the stage. This means that, for instance, the `init` property of the stage is executed only once at the beginning of the stage, regardless of how many rounds and steps the stage has got.  


## Next

[Step Callback Functions](Step-Callback-Functions-v5)

## See Also

- The full [Stager API](Stager-API-v5)
