 - status : complete
 - version : 3.x

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/choice-manager-widget.jpeg" alt="Illustration of the ChoiceManager widget">
  <br>
  <figcaption>The ChoiceManager widget handles multiple forms elements
  together, for example <a
  href="https://github.com/nodeGame/nodegame/wiki/ChoiceTable-Widget-v3">ChoiceTable</a>
  widgets.</figcaption>
</figure>

## Introduction

The ChoiceManager widget is similar to the
[ChoiceTableGroup](ChoiceTableGroup-Widget-v3), but it is more
flexible, because it can handle different type of choice selector
elements (i.e. forms).

## Main Options

- **forms**: an array of forms or a function returning the array of
    forms. Each element in the array for forms may be:
    - an instantiated widget,
    - a "widget-like" element (`append` and `getValues` methods must exist),
    - an object with the `name` of the widget and optional settings, e.g.:           
    ```js
    {
        name: 'ChoiceTable',
        mainText: 'Did you commit the crime?',
        choices: [ 'Yes', 'No' ],
    }
    ```
- **shuffleForms**: TRUE, if forms should be added in random order to
    the page.    
- **mainText**: a text to be displayed above the list
- **freeText**: if TRUE, a textarea will be added under the list,
    if 'string', the text will be added inside the the textarea.

### Additional Options

- **className**: the className of the widget (string or array), or
  false to have none.
- **group**: the name of the group (number or string), if any.
- **groupOrder**: the order of the list in the group, if any.
    
## Main methods

The main methods are the same as for
[ChoiceTable](ChoiceTable-Widget-v3).

## Return Value

The return value is an object containing an entry for each of the
forms (see Usecase section). If a selection for one or more
form is missing, and the form had the `requiredChoice` property set to
TRUE, then the return object contains the property
`missValues` equal to an array with the ids of the forms that do not
have a selection.

## Usecase

```js
var root = document.body;
var survey = node.widgets.append('ChoiceManager', root, {
    id: 'survey',
    title: false,
    shuffleForms: true,
    forms: [
        w.get('ChoiceTable', {
            id: 'age',
            mainText: 'What is your age group?',
            choices: [
                '18-20', '21-30', '31-40', '41-50',
                '51-60', '61-70', '71+', 'Do not want to say'
            ],
            title: false,
            requiredChoice: true
        }),
        w.get('ChoiceTable', {
            id: 'job',
            mainText: 'Does your current occupation involve ' +
                'regular use of artistic or creative skills? ' +
                'If so, what is your level of experience' +
                'in years?',
            choices: [
                'No', 'Yes, 1y', 'Yes, 1-2y',
                'Yes, 3-5y','Yes, 5y+', 'Do not want to say'
            ],
            title: false,
            requiredChoice: true
        }),
        w.get('ChoiceTable', {
            id: 'gender',
            mainText: 'What is your gender?',
            choices: [
                'Male', 'Female', 'Other', 'Do not want to say'
            ],
            shuffleChoices: true,
            title: false,
            requiredChoice: true
        }),
        w.get('ChoiceTable', {
            id: 'location',
            mainText: 'What is your location?',
            choices: [
                'US', 'India', 'Other', 'Do not want to say'
            ],
            shuffleChoices: true,
            title: false,
            requiredChoice: true
        })
    ]
});

// After user made his or her selection, get current values.
survey.getValues();
// Object {
//     id: "survey",
//     forms: {
//         age: {
//             id: "age"      
//             attempts: [],
//             choice: "4"
//             isCorrect: true
//             nClicks: 2,
//             time: 2555
//         },
//         gender: Object,
//         job: Object,
//         location: Object
//     },
//     missValues: [ "gender", "location" ],
//     order: [ 1, 0, 3, 2 ]    
// };

```

## Links

- [ChoiceTable](ChoiceTableGroup-Widget-v3)
- [List of Widgets](Widgets-v3)
