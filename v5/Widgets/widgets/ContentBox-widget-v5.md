 - status : complete
 - version : 5.6+


## Introduction

The ContentBox is a simple container for text, specially suited to be
used as a widget step.

## Main Options

- **mainText**: a text to be displayed above the content.

- **content**: some content to be displayed (support HTML tags).

- **hint**: a hint text below content (support HTML tags). From v5.9+

## Main methods

None.

## Return Value

None.

## Usecase

```js
// A widget step introducing a new stage.

    stager.extendStep('donesurvey', {
        name: 'The real task begins',
        widget: {
            name: 'ContentBox',
            options: {
                mainText: 'You have now finished Part 1.',
                hint: 'Relax a bit before Part 2'
                className: 'centered'
            }
        }
    });
```

## Links

- [Source Code](https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/ContentBox.js)
- [List of Widgets](Widgets-v5)
