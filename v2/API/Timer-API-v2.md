- status: complete
- version: 2.x

## Overview

The Timer API is used to create new game timers, register a timed
event, measure the time passed between two events, etc.

## Using the Timer API

### Creating Timers

The object `node.timer` is automatically instantiated whenever a
nodegame-client object is created. It contains all the methods of the
Timer API, and allows to instantiate new GameTimer objects.

A GameTimer is a configurable object to measure time lapses, time
differences, and to fire events at a specific moment in
time. GameTimers pause automatically whenever the game is paused, and
resume automatically when the game is resumed.

To create a new GameTimer use the method `createTimer`.

```javascript

var options = {

    // The name of the timer. If missing, a random name will be created.
    name: 'mytimer',
    
    // The length of the interval (in milliseconds).
    milliseconds: 4000,
    
    // How often to update the time counter (in milliseconds). 
    update: 1000,

    // An event or a function to fire when the timer expires.    
    timeup: 'MY_EVENT',
    
    // Array of functions or events to fire at every update.    
    hooks: [

        myFunc,
        
        'MY_EVENT_UPDATE',
        
        // It is possible to specify the context of execution of a function.
        { hook: myFunc2, ctx: that, }
    ]    
};

var mytimer = node.timer.createTimer(options);
```

GameTimers can be retrieved using the method `getTimer` and the name
of the timer, or directly by reference under `node.game.timer.timers`.

To destroy timers that are not needed anymore, use `destroyTimer` or
`destroyAllTimers` methods.

### Timestamps and Time Differences

It is possible to mark the time of specific event using the command
`setTimestamp` and a name of the timestamp.

```javascript
node.timer.setTimestamp('mytimestamp');
```

The command optionally takes a second parameter to set the timestamp
to a specific time, rather than `(new Date()).getTime()`.

To retrieve a timestamp later, use the command `getTimestamp` with the
name of chosen timestamp, or `getAllTimestamps` to get them all in an
object.

```javascript
node.timer.getTimestamp('mytimestamp');
```

To measure the time differences the following methods are available:

```javascript
// Time passed since a timestamp was defined.
node.timer.getTimeSince('mytimestamp');

// Time interval between two timestamps.
node.timer.getTimeDiff('mytimestamp', 'mysecondtimestamp');
```

#### Automatic Timestamps

The following timestamps are automatically created:

* **start**: time since the game has been started
* **paused**: time since the game has been paused for the last time
* **resumed**: time since the game has been resumed for the last time
* **stage** time since the game entered in the current stage  
* **step**: time since the the game entered in the current step

The timestamps above are overwritten every time the same event happens
while the game is executed. For instance, the 'step' timestamp is
overwritten at every new step.

However, the timestamp related to each specific 'step' are kept under
the name or the id of the step. That is, the timestamp related to the
beginning of step 1.1.1 is stored under '1.1.1'.

### Random Events

To fire an event or execute a callback randomly within a certain time
interval you can use the `randomEmit` and `randomExec` methods.

```javascript
// Fire an event randomly in the next 20 seconds.
node.timer.randomEmit('MYEVENT', 20000);

// Executes a callback function in the next 15 seconds.
node.timer.randomExec(myCallback, 20000);
```

## API

### Timer methods

* `createTimer`
* `destroyTimer`
* `destroyAllTimers`
* `setTimestamp`
* `getTimestamp`
* `getAllTimestamps`
* `getTimeSince`
* `getTimeDiff`
* `getTimer`
* `randomEmit`
* `randomExec`

### GameTimer Class

* `init`
* `fire`
* `start`
* `addHook`
* `removeHook`
* `pause`
* `resume`
* `stop`
* `restart`
* `isRunning`
* `isStopped`
* `isPaused`


## See Also

* [Timer class](https://github.com/nodeGame/nodegame-client/blob/master/lib/core/Timer.js)
