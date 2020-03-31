 - status : complete
 - version : 4.x

## Illustration

<figure> <img
  src="http://nodegame.org/images/wiki/choice-table-group-widget.jpeg"
  alt="Illustration of the ChoiceTableGroup widget"> <br>
  <figcaption>The ChoiceTableGroup widget groups together and manages
  sets of <a
  href="https://github.com/nodeGame/nodegame/wiki/ChoiceTable-Widget-v4">ChoiceTable</a>
  widgets.</figcaption> </figure>

## Introduction

ChoiceTableGroup creates a configurable table where each row (or
column) is composed of selectable cells of a
[ChoiceTable](ChoiceTable-Widget-v4) widget.

## Main Options

- **items**: an array of containing the ids of each choice table, or an
array of configuration objects for each choice table.
- **shuffleItems**: TRUE, if items should be added in random order to
    the page.

The remaining options are the same as for
[ChoiceTable](ChoiceTable-Widget-v4), and are inherited by each choice
table, if not overwritten inside the `items` object.

Important! Options may be different across choice tables, but some
must be the same for all:

- the `orientation` must be the same (vertical or horizontal).
- the number of available `choices`.


## Main methods
    
The main methods are the same as for
[ChoiceTable](ChoiceTable-Widget-v4).

## Return Value

The return value is an object containing an entry for each of the
items (see Usecase section). If a selection for one or more
item is missing, and the item had the `requiredChoice` property set to
TRUE, then the return object contains the property
`missValues` equal to TRUE

## Usecase

```js
// Create a choice table group where all items have same settings.
var commonOptions = {
    id: 'score',
    mainText: 'Score the item above on a scale from 0 to 10',
    items: [
            'Overall Quality',
            'Design',
            'Creativity',
            'Abstractness'
        ],
    choices: [ 1, 2, 3, 4, 5, 6, 7 ]
    shuffleItems: true, 
    requiredChoice: true,
    left: 'Lowest',
    right: 'Highest'
};           
var root = document.body;
var scoreTable = node.widgets.append('ChoiceTableGroup', root, commonOptions);


// Create a choice table with customized settings.
var i, len, items;
i = -1, len = commonOptions.items.length;
items = new Array(len);
for ( ; ++i < len ; ) {
    items[i] = {
        id: commonOptions.items[i],
        choices: commonOptions.choices,
        left: commonOptions.items[i]
    };
    if (i == 2 || i == 4) {
        item.left = '<span class="class-name">' + item.left + '</span';
    }
}
commonOptions.items = items;
var scoreTable2 = node.widgets.append('ChoiceTableGroup', root,
commonOptions);

// After user made his or her selection, get current values.
scoreTable.getValues();
// Object {
//     id: "score"
//     items: {
//         Abstractness: {
//             id: "Abstractness"
//             group: "score"
//             groupOrder: 1,
//             attempts: [],
//             choice: "0"            
//             isCorrect: true,
//             nClicks: 6,
//             time: 58824
//         },
//         Creativity: Object,
//         Design: Object,
//         Overall Quality: Object,
//         missValues: true,
//         order: [ 3, 1, 2, 0 ]
//     }
// };

```

## Links

- [ChoiceTable](ChoiceTable-Widget-v4)
- [MoodGauge](MoodGauge-Widget-v4)
- [SVOGauge](SVOGauge-Widget-v4)
- [List of Widgets](Widgets-v4)
