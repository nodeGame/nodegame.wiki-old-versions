 - status : complete
 - version : 5.x

## Illustration
<figure>
  <img src="http://nodegame.org/images/wiki/v5/VisualTimer.png" alt="Illustration of the VisualTimer widget when timing.">
  <img src="http://nodegame.org/images/wiki/v5/VisualTimerWaiting.png" alt="Illustration of the VisualTimer widget when waiting.">

  <br>
  <figcaption>Appearance of the VisualTimer widget before and after waiting has started.</figcaption>
</figure>


## Introduction

VisualTimer displays a countdown timer (less than one hour) and can periodically
trigger events.

It is responsive to `PAUSE` and `RESUME` events.

## Main Options

- **startOnPlaying**: if TRUE, at every new step (more precisely, at every new
  `PLAYING` event), the step property `timer` is read and timer restarted.
- **stopOnDone**: if TRUE, when `node.done()` is called the timer is stopped.
- **waitBoxOptions**: an option object to be passed to `TimerBox`.
- **mainBoxOptions**: an option object to be passed to `TimerBox`.

Any options that can be passed to a [GameTimer](Timer-API-v5) object is
available. In particular:

- **milliseconds**: total number of milliseconds for countdown; if undefined the
wait timer will start with the current time left.
- **timeup**: a function to call, or an event to fire when the timer expires.

## Main Methods

- **startTiming(opts)**: starts the timer and changes appearance to a regular
  countdown. The mainBox will be unstriked and set active, the waitBox will be
  hidden. The options object is passed to `VisualTimer.restart`.
- **startWaiting(opts)**: stops the timer and changes the appearance to a
  max. wait timer.  The mainBox will be striked out, the waitBox set active and
  unhidden.  This function call accepts an options object. The options object is
  passed to `VisualTimer.restart`.
- **switchActiveBoxTo**: accepts a `TimerBox` as argument, stores
  `gameTimer.timeLeft` into `activeBox` and then switches `activeBox` the
  argument.
- **setToZero**: stops the timer, sets it to zero, and strikes it.

## Listeners

The following list shows the events that a VisualTimer listens to and
how it reacts.

- `PLAYING`: if the `startOnPlaying` option is true,
  VisualTimer.startTiming is called.
- `REALLY_DONE`: if the `stopOnDone` option is true, the timer is stopped.

## Nested Classes 

- **TimerBox**: wrapper class containing the div where the timer is
  displayed. Available options:

- `hideTitle`: Flag which indicates whether to hide the title by default.
- `hideBody`: Flag which indicates whether to hide the body by default.
- `hideBox`: Flag which indicates whether to hide the title by default.
- `title`: Title of the `TimerBox`.
- `classNameTitle`: Class name of the title div.
- `classNameBody`: Class name of the body div.
- `timeLeft`: Time left initially.

## Usecase

```js
var root = W.getHeader() || document.body;
var visualTimer = node.widgets.append('VisualTimer', root);
```

## Links

- [Source Code](https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/VisualTimer.js)
- [List of Widgets](Widgets-v5)
