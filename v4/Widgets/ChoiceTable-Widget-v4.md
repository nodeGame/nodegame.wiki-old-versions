 - status : complete
 - version : 4.x

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/choice-table-widget.jpg" alt="Illustration of the ChoiceTable widget">
  <br>
  <figcaption>The ChoiceTable widget can keep one or multiple options
  selected at the same.</figcaption>
</figure>

## Introduction

ChoiceTable creates a configurable table where each cell is a
selectable choice.

## Main Options

- **orientation**: orientation of the table: vertical ('V') or
    horizontal ('H')

- **choices**: the array of available choices. Elements inside the
    array can be of type: string, number, or array. If array, the
    first element is the 'value' (incorporated in the `id` field) and
    the second the text to display as choice. However, if a custom
    `renderer` function is defined there are no restriction on the
    format.

- **selectMultiple**: if TRUE multiple cells can be selected.
    
- **correctChoice**: the array|number|string of correct choice/s.
    
- **shuffleChoices**: if TRUE, choices are shuffled before being added
    to the table.

- **left**: If set, an additional non-clickable cell is added _before_
    all clickable cells (to the left, or on top, depending on
    orientation) with the content of this option.

- **right**: If set, an additional non-clickable cell is added _after_
    all clickable cells (to the left, or on top, depending on
    orientation) with the content of this option.

- **mainText**: a text to be displayed above the table

- **freeText**: if TRUE, a textarea will be added under the table,
    if 'string', the text will be added inside the textarea
    
- **timeFrom**: The timestamp as recorded by
    [`node.timer.setTimestamp`](Timer-API-v4) or FALSE, to measure
    absolute time for current choice. Default: from the beginning of
    current step.
    

### Additional Options

- **group**: the name of the group (number or string), if any.

- **groupOrder**: the order of the table in the group, if any.

- **onclick**: a custom onclick listener function. Context is `this`
    instance.
    
- **renderer**: a function that will render the choices.

- **className**: the className of the table (string, array), or false
    to have none.

## Main methods

- **verifyChoice(markAttempt)**: compares the current choice/s with
    the correct one/s, or simply if a required choice was made. Each
    call to this method is added to the `attempts` array, unless
    parameter markAttempt is FALSE. Returns: TRUE/FALSE.

- **setCurrentChoice(choice)**: programmatically sets current
    choice/s.

- **unsetCurrentChoice(choice)**: programmatically unsets current
    choice/s.
    
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

- **setValues(opts)**: sets values in the choice table as specified by
    the options:
       - correct: If TRUE, it selects the correct answer. If FALSE or
         undefined a random selection is made. Default: FALSE.    
    
- **reset(opts)**: resets current selection and collected
    paradata. Parameter `opts` is an object that may contain:
       - shuffleChoices: If TRUE, choices are shuffled. Default: FALSE.

- **shuffle**: shuffles the order of the choices

## Return Value

The return value is an object containing the following properties:

- **id**: The id of the widget.
- **attempts**: If a correct value is specified, all attempts to guess
    it are saved in this array.
- **choice**: The current choice, that is the position in the original
    choices array.
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
```

// After user made his or her selection, get current values.
ageForm.getValues();
// Object {
//     id: "age"      
//     attempts: [],
//     choice: "4"
//     isCorrect: false
//     nClicks: 2,
//     time: 2555
// };
## Links

- [ChoiceTableGroup](ChoiceTableGroup-Widget-v4)
- [List of Widgets](Widgets-v4)
