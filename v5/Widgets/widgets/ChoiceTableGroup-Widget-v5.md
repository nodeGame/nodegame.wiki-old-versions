 - status : complete
 - version : 5.x

## Illustration

<figure> <img
  src="http://nodegame.org/images/wiki/v5/choice-table-group-widget.jpeg"
  alt="Illustration of the ChoiceTableGroup widget"> <br>
  <figcaption>The ChoiceTableGroup widget groups together and manages
  sets of <a
  href="https://github.com/nodeGame/nodegame/wiki/ChoiceTable-Widget-v5">ChoiceTable</a>
  widgets.</figcaption> </figure>

## Introduction

ChoiceTableGroup creates a configurable table where each row (or
column) is composed of selectable cells of a
[ChoiceTable](ChoiceTable-Widget-v5) widget.

## Main Options

- **items**: an array of containing the ids of each choice table, or an
array of configuration objects for each choice table.
- **shuffleItems**: TRUE, if items should be added in random order to
    the page.
- **onclick**: a custom onclick listener function. Receives 4 input
  parameters: the id of the choicetable clicked, value of the clicked
  choice, whether it was a remove action, and the reference to the
  corresponding TD object; context is `this` instance.

The remaining options are mainly the same as for
[ChoiceTable](ChoiceTable-Widget-v5) and are inherited by each choice
table (if not overwritten inside the `items` object).

The following constraints apply:

- `orientation`: must be the same (vertical or horizontal),
- `choices`: length must be the same,

The following options are not supported:

- `left`, `right`, `choicesSetSize`.

## Main methods

The main methods are the same as for
[ChoiceTable](ChoiceTable-Widget-v5).

## Return Value

The return value is an object containing an entry for each of the
items (see Usecase section). If a selection for one or more
item is missing, and the item had the `requiredChoice` property set to
TRUE, then the return object contains the property
`missValues` equal to TRUE

## Usecase

Create a widget to score images on several dimensions on a Likert scale 1-7.

#### 1. Create a choice table group where all items have the same settings.

```js
var options = {
    id: 'score',
    mainText: 'Score the item above on a scale from 0 to 10',
    items: [
            'Overall Quality',
            'Design',
            'Creativity',
            'Abstractness'
        ],
    choices: [ 1, 2, 3, 4, 5, 6, 7 ],
    shuffleItems: true,
    requiredChoice: true,
    left: 'Lowest',
    right: 'Highest'
};
// Create and append the widget to the body of the page.
var root = document.body;
var scoreTable = node.widgets.append('ChoiceTableGroup', root, options);
```
#### 2. Create another widget where some items have different settings.

```js
var i, len, items;
i = -1, len = options.items.length;
// Create a new array of items to replace the original one.
items = new Array(len);
for ( ; ++i < len ; ) {
    // Instead of a string containing the id of the choice table,
    // we pass an object specifying the all the options.
    items[i] = {
        id: options.items[i],
        choices: options.choices,
        left: options.items[i]
    };
    // Here we assign a different class name for choice table 2 and 4.
    if (i == 2 || i == 4) {
        item.left = '<span class="class-name">' + item.left + '</span>';
    }
}
// Updating the items in the options, and creating the new choice table group.
options.items = items;
var scoreTable2 = node.widgets.append('ChoiceTableGroup', root, options);

// After user made his or her selection, get current values.
scoreTable2.getValues();
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

// For example, choices on the items can be read as follows:
var Val = scoreTable2.getValues();
var Choice = Val.items["Overall Quality"].choice; // choice on the 'Overall quality' item
```

## Links

- [Source Code](https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/ChoiceTableGroup.js)
- [ChoiceTable](ChoiceTable-Widget-v5)
- [List of Widgets](Widgets-v5)
