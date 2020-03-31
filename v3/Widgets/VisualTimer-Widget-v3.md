 - status : complete
 - version : 3.x

## Illustration
<figure>
  <img src="http://nodegame.org/images/wiki/VisualTimer.png" alt="Illustration of the VisualTimer widget when timing.">
  <img src="http://nodegame.org/images/wiki/VisualTimerWaiting.png" alt="Illustration of the VisualTimer widget when waiting.">

  <br>
  <figcaption>Appearance of the VisualTimer widget before and after waiting has started.</figcaption>
</figure>


## Introduction

VisualTimer is a [widget](widgets-api) which displays a countdown
(less than one hour). The timer can trigger events.


## Construction

VisualRound provides a constructor which accepts an options object.

### Options

The options object can have the following attributes:
- any options that can be passed to a [GameTimer](gametimer-api)
- `waitBoxOptions`: an option object to be passed to `TimerBox`
- `mainBoxOptions`: an option object to be passed to `TimerBox`

If `waitBoxOptions` or `mainBoxOptions` are undefined the widget is
initialized with two `TimerBox`s representing maximal waiting time and
remaining time to complete task respectively.

## Selected Methods

### VisualTimer.startWaiting

Stops the timer and changes the appearance to a max. wait timer.
The mainBox will be striked out, the waitBox set active and unhidden. 
This function call accepts an options object. 

#### Options

The options object can specify the following attributes:

- `milliseconds` Where to start the wait time countdown. If undefined
the wait timer will start with the current time left on the
`gameTimer`.
- any options that can be passed to a [GameTimer](gametimer-api)
- `waitBoxOptions`: an option object to be passed to `TimerBox`
- `mainBoxOptions`: an option object to be passed to `TimerBox`

All other options are forwarded directly to `VisualTimer.restart`.

### VisualTimer.startTiming

Starts the timer and changes appearance to a regular countdown. 

The mainBox will be unstriked and set active, the waitBox will be
hidden. The accepted options object is forwarded directly to
`VisualTimer.restart`.


### VisualTimer.switchActiveBoxTo

Accepts a `TimerBox` as argument, stores `gameTimer.timeLeft` into
`activeBox` and then switches `activeBox` the argument.

## Listeners

The following list shows the events that a VisualTimer listens to and
how it reacts.

- `PLAYING`: if the `startOnPlaying` option is true,
  VisualTimer.startTiming is called.
- `REALLY_DONE`: if the `stopOnDone` option is true,
  VisualTimer.startWaiting is called.

## TimerBox

`TimerBox` is the javascript class of the box where the timer is displayed

### Options

The options object can have the following attributes:

- `hideTitle`: Flag which indicates whether to hide the title by default.
- `hideBody`: Flag which indicates whether to hide the body by default.
- `hideBox`: Flag which indicates whether to hide the title by default.
- `title`: Title of the `TimerBox`.
- `classNameTitle`: Class name of the title div.
- `classNameBody`: Class name of the body div.
- `timeLeft`: Time left initially.

## Links

- [List of Widgets](Widgets-v3)
