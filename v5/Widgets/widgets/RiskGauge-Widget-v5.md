 - status : complete
 - version : 5.x, 5.6+, 5.12+

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/v5/risk-gauge.png" alt="Illustration of the SVOGauge widget">
  <br>
  <figcaption>Measurament of risk preferences with the Holt and Laury
  method (only first 3 choices displayed).</figcaption>
</figure>

## Introduction

The RiskGauge measures
[risk preferences](https://en.wikipedia.org/wiki/Risk_aversion). There are two
elicitations methods available (gauges):

- _"Holt and Laury"_ (default): a series of lottery questions. Reference: <a
href="https://www.aeaweb.org/articles?id=10.1257/000282802762024700"
target="_blank">Holt and Laury (2005)</a>.

- _"Bomb"_ (v5.12.0+): a decision on how many boxes to open, but one could be a bomb. Reference: <a href="https://link.springer.com/article/10.1007/s11166-013-9170-z" target="_blank">Crosetto and Filippin (2013)</a>.


## Main Options

### Holt and Laury

Options available from v5.6.2+:

- **values**: array of values using to create the Holt and Laury
  lotteries. Default: [ 2, 1.6, 3.85, 0.1 ].
- **scale**: a number multiplying the values. Default: 1.
- **currency**: The currency symbol. Default: '$'.
- **of**: String following the chance fraction and preceding the winning outcome. Default: ' chance to win '.
- **sep**: separator between the two options in one lottery. Default:
  `<span class="sep">and</span>`.

### Bomb

  - **boxHeight**: a string containing a CSS rule for height of each box. Default: '30px'.
  - **probBomb**: probability that there is a bomb. Default: 1.
  - **revealProbBomb**: if TRUE, the actual probability that there is a bomb is revealed in the description of the task. Default: TRUE.
  - **boxValue**: reward for each opened box. Default: 0.01.
  - **totBoxes**: number of boxes to open. Default: 100.
  - **maxBoxes**: number of boxes that can be opened. Default: `totBoxes` - 1 if `probBomb` = 1, otherwise `totBoxes`.
  - **boxesInRow**: number of boxes to display in each row. Default: 10.
  - **withPrize**: if TRUE, the task is incentivized. Default: TRUE.
  - **currency**: The currency symbol. Default: 'USD'.

## Texts

### Holt and Laury

 - **holt_laury_mainText**: main text for Holt and Laury gauge.
 - **mainText**: backward-compatible alias.

### Bomb

 - **bomb_mainText**: main text for the Bomb gauge,
 - **bomb_sliderHint**: text above the slider,
 - **bomb_boxValue**: prize per box,
 - **bomb_numBoxes**: number of boxes,
 - **bomb_totalWin**: total reward,
 - **bomb_openButton**: text on the button to open the boxes,
 - **bomb_warn**: warning to open at least one box,
 - **bomb_won**: message for users who did not find the bomb,
 - **bomb_lost**: message for users who found the bomb.

## Return Value

### Holt and Laury

An object containing an entry for each choice. If a selection for one or more choice is missing, the object contains the property `missValues` equal to TRUE (see Usecase section for details).

### Bomb

An object containing the number of boxes chosen in the `value` property. If the user did not click on the 'Open Boxes' button yet, the object contains the property `isCorrect` equal to FALSE (see Usecase section for details).


## Usecase


### Holt and Laury

```js
// Creates and append a new RiskGauge widget with Holt and Laury gauge.
var root = document.body;
var risk = node.widgets.append('RiskGauge', root, {
    currency: 'ECU' // Overwrites the dollar sign.
});

// After userGet current values.
risk.getValues();
// {
//   "id": "holt_laury",
//   "order": [
//     0,
//     1,
//     2,
//     3,
//     4,
//     5,
//     6,
//     7,
//     8,
//     9
//   ],
//   "items": {
//     "hl_1": {
//       "id": "hl_1",
//       "choice": "1",
//       "time": 2740,
//       "nClicks": 1,
//       "value": "1/10 chance to win ECU0.5<span class=\"sep\">and</span>9/10 chance to win ECU0.10",
//       "group": "holt_laury",
//       "groupOrder": 1,
//       "isCorrect": true,
//       "attempts": []
//     },
//     "hl_2": {
//       "id": "hl_2",
//       "choice": "0",
//       "time": 3683,
//       "nClicks": 1,
//       "value": "2/10 chance to win ECU2.00<span class=\"sep\">and</span>8/10 chance to win ECU1.60",
//       "group": "holt_laury",
//       "groupOrder": 2,
//       "isCorrect": true,
//       "attempts": []
//     },
//     "hl_3": {
//       "id": "hl_3",
//       "choice": "0",
//       "time": 4057,
//       "nClicks": 1,
//       "value": "3/10 chance to win ECU2.00<span class=\"sep\">and</span>7/10 chance to win ECU1.60",
//       "group": "holt_laury",
//       "groupOrder": 3,
//       "isCorrect": true,
//       "attempts": []
//     },
//     "hl_4": {
//       "id": "hl_4",
//       "choice": "0",
//       "time": 4657,
//       "nClicks": 1,
//       "value": "4/10 chance to win ECU2.00<span class=\"sep\">and</span>6/10 chance to win ECU1.60",
//       "group": "holt_laury",
//       "groupOrder": 4,
//       "isCorrect": true,
//       "attempts": []
//     },
//     "hl_5": {
//       "id": "hl_5",
//       "choice": "1",
//       "time": 5605,
//       "nClicks": 1,
//       "value": "5/10 chance to win ECU3.85<span class=\"sep\">and</span>5/10 chance to win ECU0.10",
//       "group": "holt_laury",
//       "groupOrder": 5,
//       "isCorrect": true,
//       "attempts": []
//     },
//     "hl_6": {
//       "id": "hl_6",
//       "choice": "1",
//       "time": 5929,
//       "nClicks": 1,
//       "value": "6/10 chance to win ECU3.85<span class=\"sep\">and</span>4/10 chance to win ECU0.10",
//       "group": "holt_laury",
//       "groupOrder": 6,
//       "isCorrect": true,
//       "attempts": []
//     },
//     "hl_7": {
//       "id": "hl_7",
//       "choice": "0",
//       "time": 6666,
//       "nClicks": 1,
//       "value": "7/10 chance to win ECU2.00<span class=\"sep\">and</span>3/10 chance to win ECU1.60",
//       "group": "holt_laury",
//       "groupOrder": 7,
//       "isCorrect": true,
//       "attempts": []
//     },
//     "hl_8": {
//       "id": "hl_8",
//       "choice": "1",
//       "time": 7002,
//       "nClicks": 1,
//       "value": "8/10 chance to win ECU3.85<span class=\"sep\">and</span>2/10 chance to win ECU0.10",
//       "group": "holt_laury",
//       "groupOrder": 8,
//       "isCorrect": true,
//       "attempts": []
//     },
//     "hl_9": {
//       "id": "hl_9",
//       "choice": "0",
//       "time": 7897,
//       "nClicks": 1,
//       "value": "9/10 chance to win ECU2.00<span class=\"sep\">and</span>1/10 chance to win ECU1.60",
//       "group": "holt_laury",
//       "groupOrder": 9,
//       "isCorrect": true,
//       "attempts": []
//     },
//     "hl_10": {
//       "id": "hl_10",
//       "choice": "1",
//       "time": 8571,
//       "nClicks": 1,
//       "value": "10/10 chance to win ECU3.85<span class=\"sep\">and</span>0/10 chance to win ECU0.10",
//       "group": "holt_laury",
//       "groupOrder": 10,
//       "isCorrect": true,
//       "attempts": []
//     }
//   },
//   "isCorrect": true
// }
```

### Bomb

```js
// Creates and append a new RiskGauge widget with Holt and Laury gauge.
var root = document.body;
var risk = node.widgets.append('RiskGauge', root, {
        method: 'Bomb',
        title: false,
        currency:'EUR',
        probBomb: 0.5,
        revealProbBomb: false,
        texts: {
            buttonText: 'BOOOOM',
        }
    }
});

risk.getValues();

{
    value: 34,        // number of boxes opened.
    isCorrect: true,  // TRUE, if the user has clicked on the 'Open boxes' button
    isWinner: false,  // TRUE, if the user did not find the bomb after clicking on the 'Open boxes' button.
    reward: 0,        // Total reward for the user.
    time: 4444609,    // Time in milliseconds from the creation of the widget
    totalMove: 57    // Total movement of the slider.
}
```

## Links

- [Source Code](https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/RiskGauge.js)
- [List of Widgets](Widgets-v5)
