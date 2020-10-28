 - status : complete
 - version : 5.6+

## Illustration
<figure>
  <img src="http://nodegame.org/images/wiki/v5/custominput-group.png" alt="Illustration of the CustomInput type 'date' widget">
  <br>
  <figcaption>CustomInputGroup used to measure preferences for social mobility.</figcaption>
</figure>

## Introduction

The CustomInputGroup widget groups together and handles multiple
  [CustomInput](CustomInput-Widget-v5] widgets.
  
## Main Options

- **orientation**: `vertical` (shorthand `v`), `horizontal` (shorthand `h`).
  
- **items**: An array of objects containing the configuration options
  for each individual CustomInput. Each object should contain at least
  an `id` and and `mainText` property. 

- **sharedOptions**: An object containing options shared with all
   CustomInputs in the items array.

- **summary**: An object containing the configuration options for a
  summary field (displayed after the last CustomInput); if of type
  `string`, the object `{ mainText: summary }` will be created.

- **oninput**: A callback called when any input is changed. It 
  It is executed after validation of the input, and receives three
  input parameters: the result of the input validation (see
  [CustomInput](CustomInput-Widget-v5), a reference to the CustomInput
  object, and a reference to this widget. 
  
- **validation**: A validation callback for all inputs. By default, it
  is called when `getValues` is executed, but not ony input change. It
  receives three input parameters: an object containing the widget's
  current values (as in `getValues`), an object containing the current
  values of each input, and a reference to this widget.

### Survey Widgets Options

- **requiredChoice**: If TRUE, input is required.

- **mainText**: A text to be displayed above the form.

- **hint**: A smaller text to be displayed after the main text.


## Main methods

- **setError(err)**: Sets an error message and highlights the widget

- **setValues(opts)** Sets the values of the form. Available options:
  
    - **items**: The array of values for each custom input (see
  `setValues` options for [CustomInput](CustomInput-Widget-v5)).
    - **textarea**: Value for the textarea.
        
    If not option is specified, a completely random value consistent with the
    type is chosen. 

### getValues Return Value

The return value is an object containing the following properties:

- **id**: The id of the widget.

- **timeBegin**: the time passed in milliseconds from the beginning of the step
    until first character was typed in the form.
    
- **timeEnd**: the time passed in milliseconds from the beginning of the step
    until last character was typed in the form.

- **value**: the validated and postprocessed value of the input.

- **isCorrect**: TRUE, if there are no errors. This property is not added if
  getValues is called with property markAttempt = FALSE.

## Usecase

```js
var root = W.getElementById('root');

var somo = node.widgets.append('CustomInputGroup', root, {
    id: id + '_widget',
    panel: false,
    orientation: 'H',
    mainText: 'What is the likelihood ' +
        'that a person would be in each of the ' +
        'following income groups as an adult?',
    hint: '(Assign numbers between 0 and 100 to each ' +
        'group, the total must be equal to 100)',
    sharedOptions: {
        type: 'number',
        min: 0,
        max: 100,
        width: '80%'
    },
    requiredChoice: true,
    summary: 'Total',
    items: [
        {
            id: 'poorest',
            mainText: 'Poorest 20%'
        },
        {
            id: 'poor',
            mainText: 'Poor 20%'
        },
        {
            id: 'mid',
            mainText: 'Mid 20%'
        },
        {
            id: 'rich',
            mainText: 'Rich 20%'
        },
        {
            id: 'richest',
            mainText: 'Richest 20%'
        }
    ],
    validation: function(res, values, that) {
        var sum;
        sum = (J.isNumber(values.poorest) || 0) +
            (J.isNumber(values.poor) || 0) +
            (J.isNumber(values.mid) || 0) +
            (J.isNumber(values.rich) || 0) +
            (J.isNumber(values.richest) || 0);
        that.summaryInput.setValues({ values: sum });
        if (sum !== 100) {
            res.err = 'Total must be equal to 100';
        }
        return res;
    },
    oninput: function(res, input, that) {
        that.validation();
    }
});

// Get current values.
somo.getValues();

// {
//   "id": "social_mobility_widget",
//   "order": [
//     0,
//     1,
//     2,
//     3,
//     4
//   ],
//   "items": {
//     "poorest": {
//       "value": 10,
//       "timeBegin": 2248415,
//       "timeEnd": 2248506,
//       "isCorrect": true,
//       "id": "poorest"
//     },
//     "poor": {
//       "value": 20,
//       "timeBegin": 2248906,
//       "timeEnd": 2249037,
//       "isCorrect": true,
//       "id": "poor"
//     },
//     "mid": {
//       "value": 30,
//       "timeBegin": 2249816,
//       "timeEnd": 2249922,
//       "isCorrect": true,
//       "id": "mid"
//     },
//     "rich": {
//       "value": 40,
//       "timeBegin": 2250758,
//       "timeEnd": 2250863,
//       "isCorrect": true,
//       "id": "rich"
//     },
//     "richest": {
//       "value": 0,
//       "timeBegin": 2252340,
//       "timeEnd": 2252340,
//       "isCorrect": true,
//       "id": "richest"
//     }
//   },
//   "isCorrect": true
// }

```

## Links

- [Source Code](https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/CustomInputGroup.js)
- [Custom Input Widget](CustomInput-Widget-v5)
- [List of Widgets](Widgets-v5)
