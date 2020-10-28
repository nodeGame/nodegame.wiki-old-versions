 - status : complete
 - version : 5.x

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/v5/mood-gauge-widget.jpeg" alt="Illustration of the MoodGauge widget">
  <br>
  <figcaption>The MoodGauge measures the mood of a participant.</figcaption>
</figure>

## Introduction

The MoodGauge measures the mood of a participant. The default
measurement (gauge) is the _"International Positive And Negative
Affection Schedule (Short Form),"_ as found in <a
href="http://journals.sagepub.com/doi/abs/10.1177/0022022106297301"
target="_blank">Thompson (2007)</a>.

## Main Options

- **emotions**: is an array that specifies the emotions to test
    instead of the default ones.
    
## Return Value

The return value is an object containing an entry for each of the
emotions (see Usecase section). If a selection for one or more
emotions is missing, then the return object contains the property
`missValues` equal to TRUE.

## Usecase

```js
// Create and append MoodGauge.
var root = document.body;
var mood = node.widgets.get('MoodGauge', root);

// After user made his or her selection, get current values.
mood.getValues();
// Object {
//     id: "ipnassf"
//     items: {
//         Active: {
//             id: "Active",
//             group: "ipnassf",
//             groupOrder: 10,
//             attempts: [],
//             choice: "3",
//             isCorrect: true,
//             nClicks: 1,
//             time: 9510
//         },
//         Afraid: Object
//         Alert: Object
//         Ashamed: Object
//         Attentive: Object
//         Determined: Object
//         Hostile: Object
//         Inspired: Object
//         Nervous: Object
//         Upset: Object
//    },
//    missValues: true,
//    order: [ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 ]
// };

```

## Links

- [Source Code](https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/MoodGauge.js)
- [List of Widgets](Widgets-v5)
