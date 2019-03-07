# VisualRound Widget
 - status : complete
 - version : 1.x

## Illustration
<figure>
  <img src="http://nodegame.org/images/wiki/VisualRound.png" alt="Illustration of the VisualRound widget">
  <br>
  <figcaption>Appearance of the VisualRound widget when configured to count up the rounds and stages to their respective totals.</figcaption>
</figure>

## Introduction

VisualRound is a [widget](widgets-api) to display information about
the current round and stage as well as total number of rounds and
stages in various ways.

## Construction

VisualRound provides a constructor which accepts an options object.

### Options

The options object can have the following attributes:
- `stageOffset`:
The value for current stage is reduced by `stageOffset`
- `totStageOffset`:
The value for total number of stages is reduced by `totStageOffset` (if
not set, it is equal to `stageOffset`)
- `flexibleMode`:
Set `true`, if number of rounds and/or stages can change dynamically
- `curStage`:
When (re)starting in `flexibleMode`, sets the current stage
- `curRound`:
 When (re)starting in `flexibleMode`, sets the current round
- `totStage`:
When (re)starting in `flexibleMode`, sets the total number of stages
- `totRound`:
 When (re)starting in `flexibleMode`, sets the total number of
rounds
- `oldStageId`:
When (re)starting in `flexibleMode`, sets the id of the current
stage
- `displayModeNames`:
Array of strings which determines the display style(s) of the widget

### Display Modes

The following strings are valid strings for the `displayModeNames` array:
- `COUNT_UP_STAGES`: Display only current stage number.
- `COUNT_UP_ROUNDS`: Display only current round number.
- `COUNT_UP_STAGES_TO_TOTAL`: Display current and total stage number.
- `COUNT_UP_ROUNDS_TO_TOTAL`: Display current and total round number.
- `COUNT_DOWN_STAGES`: Display number of stages left to play.
- `COUNT_DOWN_ROUNDS`: Display number of rounds left in this stage.

In `flexibleMode` the total stage and round number is unknown
wherefore it is represented by a question mark.

## Setting the display mode

After construction of a VisualRounds object the display mode can be
changed by using `VisualRound.setDisplayMode` which accepts an array
of display mode names such as the `displayModeNames` option of the
constructor.

## Usecase
```js
/*
 * Create and add a widget to the root which displays the 
 * current and total stage numbers minus one.
 */
var root = W.getElementById('myRoot');
var visualRounds = node.widgets.append('VisualRound', root, {
    displayModeNames: ['COUNT_UP_STAGES_TO_TOTAL'], 
    stageOffset: 1
});

// ...

// Start displaying round information as well.
visualRounds.setDisplayMode(['COUNT_UP_STAGES_TO_TOTAL',
                             'COUNT_UP_ROUNDS_TO_TOTAL']);
```
