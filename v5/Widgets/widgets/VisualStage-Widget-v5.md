 - status : complete
 - version : 5.x

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/v5/visualstage-table.png"
  alt="Illustration of the VisualStage widget in 'table' mode">
</figure>
<figure>
  <img src="http://nodegame.org/images/wiki/v5/visualstage-inline.png"
  alt="Illustration of the VisualStage widget in 'inline' mode">
</figure>

## Introduction

The VisualStage displays the name of previous, current, and next stage.

The name of a step is the value of the step property `name`, or if not found the
`id` of the step.

## Main Options

- **displayMode**: how the step names are displayed: 'inline' or
'table'. Default: 'inline'.
- **current**: if TRUE, the name of the current step is displayed. Default: TRUE.
- **next**: if TRUE, the name of the next step is displayed. Default: TRUE.
- **previous**: if TRUE, the name of the previous step is displayed. Default: TRUE.
- **rounds**: if TRUE, round numbers are displayed after the step name, but only
  if the stage has more than 1 step.
- **order**: the order in which current, next, and previous step names are
  displayed. Default: [ 'current', 'next', 'previous' ] for displayMode 'table',
  and [ 'previous', 'current', 'next' ] for displayMode 'inline'.
- **capitalize**: If TRUE, the name of the step is capitalize (applies to every
  word in the step name). Default: TRUE.
 - **replaceUnderscore**: If TRUE, underscores in the name of the step are replaced with spaces. Default: TRUE. (from _v5.10.1_+)
- **preprocess**: An optional callback transforming the name of the step before
  displaying it. It is applied _after_ the round count is added. Example:
  ```javascript
  function preprocess(name, mod, round) {
     // name is the name of the step.
     // mod may be 'current', 'previous', 'next'.
     // round is the step round or  undefined.
     return name + ' (tutorial)';
  }
  ```

## Main methods

- **updateDisplay()**: updates the display.
- **getStepName(gameStage, curStage, mod)**: returns the name of the step,
    including the round number.

## Listeners

- **STEP_CALLBACK_EXECUTED**: updates the display.

## Usecase

```js
// Creates and append a new VisualStage widget.
var root = W.getHeader();
var options = {
    rounds: true,
    previous: false
};
var vs = node.widgets.append('VisualStage', root, options);

```

## Links

- [Source Code](https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/VisualStage.js)
- [List of Widgets](Widgets-v5)
