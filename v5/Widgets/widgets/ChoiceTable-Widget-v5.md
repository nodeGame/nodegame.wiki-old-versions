 - status : complete
 - version : 5.x

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/v5/choice-table-widget.jpg" alt="Illustration of the ChoiceTable widget">
  <br>
  <figcaption>The ChoiceTable widget can keep one or multiple options
  selected at the same.</figcaption>
</figure>

## Introduction

ChoiceTable creates a configurable table where each cell is a
selectable choice.

## Main Options

- **orientation**: orientation of the table: vertical ('V') or
    horizontal ('H').

- **choices**: the array of available choices. It may contain elements
    of type: string, number, HTML element, or array. If array, the
    first element is the value (string or number), and the second one
    is the display (string, number or HTML element). However, if a
    custom `renderer` function is defined there are no restriction on
    the format of choices. As of nodeGame v5.6.4+, choices can also
    contain objects of the type: `{ value: 'x', display: 'y' }`.

- **requiredChoice**: how many choices must selected (TRUE=1); this is
    a lower bound, an higher number of select choices is allowed, as
    specified by `selectMultiple` (you need to specify a value for
    `selectMultiple` for any value of `requiredChoice` > 1).

- **selectMultiple**: how many choices can be selected at the same time
    (TRUE=unlimited); set it equal to `requiredChoice` to have an exact number
    of choices to be selected.

- **correctChoice**: the array|number|string of correct choice/s.

- **shuffleChoices**: if TRUE, choices are shuffled before being added
    to the table.

- **left**: If set, an additional non-clickable cell is added _before_
    all clickable cells (to the left, or on top, depending on
    orientation) with the content of this option.

- **right**: If set, an additional non-clickable cell is added _after_
    all clickable cells (to the left, or on top, depending on
    orientation) with the content of this option.

- **mainText**: a text to be displayed above the table.

- **freeText**: if TRUE, a textarea will be added under the table,
    if 'string', the text will be added inside the textarea.

- **timeFrom**: The timestamp as recorded by
    [`node.timer.setTimestamp`](Timer-API-v5) or FALSE, to measure
    absolute time for current choice. Default: from the beginning of
    current step.

- **onclick**: a custom onclick listener function. Receives 3 input
  parameters: the value of the clicked choice, whether it was a remove
  action, and the reference to the corresponding TD object; context is
  `this` instance.

- **separator**: a string used to delimit id and choice number inside the DOM.
  Default: "::".



### Additional Options

- **group**: the name of the group (number or string), if any.

- **groupOrder**: the order of the table in the group, if any.

- **renderer**: A callback function that fills in the content of each
         table cell. It receives three input parameters:

     - A td HTML element,
     - A choice string/object
     - The index of the choice element within the choices array

    If may optionally return the _value_ for the choice (otherwise
    the order in the choices array is used as value).

- **className**: the className of the table (string, array), or false
    to have none.

- **listener**: the main onclick listener, handling all the update
  operations and finally calling the custom onclick listener.

  - **oneTimeClick**: If TRUE, a selected choice is immediately deselected;
  useful to create a buttons group. Default: FALSE. From v5.8.1+

## Main methods

- **verifyChoice(markAttempt)**: compares the current choice/s with
    the correct one/s, or simply if a required choice was made. Each
    call to this method is added to the `attempts` array, unless
    parameter markAttempt is FALSE. Returns: TRUE/FALSE.

- **setCurrentChoice(choice)**: programmatically sets current
    choice/s. Notice: it does not update the display.

- **unsetCurrentChoice(choice)**: programmatically unsets current
    choice/s. Notice: it does not update the display.

- **isChoiceCurrent(choice)**: TRUE is a choice is selected.

- **getChoiceAtPosition(position)**: returns the choice which is at a
    given position (useful when choices are displayed randomly).

- **getValues(opts)**: returns the values for current selection and
    other paradata. Parameter `opts` optionally configures the return
    value:

    - markAttempt: if TRUE, getting the value counts as an attempt
         to find the correct answer. Default: TRUE.
    - highlight: If TRUE, if current value is not the correct
         value, widget will be highlighted. Default: FALSE.
    - reset: If TRUE and a correct choice is selected (or not
         specified), then it resets the state of the widgets before
         returning. If object, it is passed directly to the `reset`
         method. Default: FALSE.
    - processChoice: a function that receives in input a choice and
         returns its updated value.
    - addValue: if FALSE, it does not add a .value property. Default
        TRUE.
    - sortValue: If TRUE and multiple choices are allowed, the values
        in the `.value` property are sorted alphabetically. Note! The
        choices array is not sorted. Default: TRUE.

- **setValues(opts)**: sets values in the choice table as specified by
    the options:
       - correct: If TRUE, it selects the correct answer. If FALSE or
         undefined a random selection is made. Default: FALSE.

- **reset(opts)**: resets current selection and collected
    paradata. Parameter `opts` is an object that may contain:
       - shuffleChoices: If TRUE, choices are shuffled. Default: FALSE.

- **shuffle()**: shuffles the order of the choices.

## Return Value

The return value is an object containing the following properties:

- **id**: The id of the widget.
- **attempts**: If a correct value is specified, all attempts to guess
    it are saved in this array.
- **choice**: The current choice/s, that is the position/s in the original
    choices array.
- **value**: The current value/s, that is the display text or the
    value specified in the choices array.
- **isCorrect**: if a correct choice is specified, or simply any choice
    is required, this variable is TRUE if the requirement is satisfied.
- **nClicks**: the total number of clicks across all choices
- **time**: the time passed in milliseconds from the event specified
    in property `timeFrom`.


## Usecase

```js
var root = W.getElementById('root');

var ageForm = node.widgets.append('ChoiceTable', root, {
    id: 'age',
    mainText: 'What is your age group?',
    choices: [
        '18-20', '21-30', '31-40', '41-50',
        '51-60', '61-70', '71+', 'Do not want to say'
    ],
    title: false,
    requiredChoice: true
});

// After user made his or her selection, get current values.
ageForm.getValues();
// Object {
//     id: "age",
//     attempts: [],
//     choice: "4"
//     isCorrect: true,
//     value:  "51-60",
//     nClicks: 2,
//     time: 2555
// };


// Example with custom value different from display.

var socialMediaForm = node.widgets.append('ChoiceTable', root, {
    id: 'socialmedia',
    mainText: 'How much time do you spend on social media?',
    choices: [
        [ 'very', 'I am a very active user' ],
        [ 'somewhat', 'I am a somewhat active user' ],
        [ 'rarely', 'I rarely use them' ],
        [ 'never', 'I never use them' ]
   ]
});

ageForm.getValues();
// Object {
//     id: "socialmedia",
//     attempts: [],
//     choice: "1"
//     isCorrect: true,
//     value:  "somewhat",
//     nClicks: 1,
//     time: 1000
// };

```

## Links

- [Source Code](https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/ChoiceTable.js)
- [ChoiceTableGroup](ChoiceTableGroup-Widget-v5)
- [List of Widgets](Widgets-v5)
