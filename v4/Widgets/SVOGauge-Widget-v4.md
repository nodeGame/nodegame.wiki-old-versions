 - status : complete
 - version : 4.x

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/svo-gauge-widget.jpeg" alt="Illustration of the SVOGauge widget">
  <br>
  <figcaption>The SVOGauge measures the Social Value Orientation
  (S.V.O.).</figcaption>
</figure>

## Introduction

The SVOGauge measures the level of
[social value orientation (S.V.O.)](https://en.wikipedia.org/wiki/Social_value_orientations)
of a participant. The default measurement (gauge) is the _"Slider
Measure," as found in <a
href="http://journal.sjdm.org/11/m25/m25.html" target="_blank">Murphy
et Al. (2011)</a>.

## Main Options

As in [ChoiceTableGroup](ChoiceTableGroup-Widget-v4). However, options
`title` and `items` cannot be changed. Optionally:

- **sliders**: is an array of arrays, where each nested element
    specifies the values to split between self and other for each
    choice.

## Main methods
    
As in [ChoiceTableGroup](ChoiceTableGroup-Widget-v4).

## Return Value

The return value is an object containing an entry for each of the
sliders (see Usecase section). If a selection for one or more
slider is missing, then the return object contains the property
`missValues` equal to TRUE.

## Usecase

```js
// Creates and append a new SVOGauge widget.
var root = document.body;
var svo = node.widgets.append('SVOGauge', root);

// After userGet current values.
svo.getValues();
// Object {
//     id: "svo_slider"
//     items: { 
//         "1": {
//             id: "1",
//             group: "svo_slider",
//             groupOrder: 1,
//             attempts: Array[0],
//             choice: [ 85, 15 ],
//             isCorrect: true,
//             nClicks: 1,
//             time: 1537
//         },
//         "2": Object,
//         "3": Object,
//         "4": Object,
//         "5": Object,
//         "6": Object
//     },
//     missValues: true,
//     order: [ 0, 1, 2, 3, 4, 5 ]
// };
```

## Links

- [ChoiceTableGroup](ChoiceTableGroup-Widget-v4)
- [List of Widgets](Widgets-v4)
