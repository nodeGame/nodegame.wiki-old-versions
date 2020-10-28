 - status : complete
 - version : 5.9+

## Illustration

<figure>
  <img width=500 src="http://nodegame.org/images/wiki/v5/slider-widget.png" alt="Illustration of the Slider widget">
  <br>
  <figcaption>The slider widget as a real-effort task (type=volume).</figcaption>
</figure>

## Introduction

The Slider widget creates a customizable slider.

## Main Options

- **mainText**: a text to be displayed above the slider.
- **hint**: an extra informative text, after the main text.
- **type**: the type of slider: "flat", "volume" (default).
- **min**: the minimum (left-most) value of the slider. Default: 0.
- **max**: the maximum (tight-most) value of the slider. Default: 100.
- **initialValue**: the initial position of the slider, or "random" for a random position. Default: 50.
- **correctValue**: if set, this value is used to determined whether the final position of the slider is correct. Default: null
- **displayValue**: if TRUE, the current numeric value of the slider is displayed. Default: TRUE.
- **displayNoChange**: if TRUE, a checkbox for "no change" is added; when pressed the slider is reset to the initial value. Default: TRUE.
- **required**: if TRUE, it is marked as a required choice.
- **onmove**: a callback function executed at every move. See usecase for details.


### Additional Options

- **className**: the className of the widget (string or array), or
  false to have none.
- **required**: If TRUE, slider movement is required, and the correct value ( if set) must match the current value.

### Texts

- **currentValue**: Formats the display of the current value. Default:

```js
function(widget, value) {
    return 'Value: ' + value;
}
```

- **nochange**: The label for the noChange checkbox. Default: 'No change'.

## Return Value

```js
{
    value: 69,
    noChange: true,
    initialValue: 10,
    isCorrect: true, // if correctValue is set value must match it    
    totalMove: 245, // the amount of moves in absolute value,
    time: 3000 // since beginning of step.
}
```
## Usecase

```js
var root = document.body;
var slider = node.widgets.append('Slider', root, {
    id: 'myslider',
    initialValue: 25,
    correctValue 89,
    displayValue: false,
    mainText: 'Move the slider to position 89',
    hint: 'Be precise!',
    required: true,
    onmove: function(value, diff) {
        console.log('Slider moved to ' + value + ' from ' + (value - diff));
    }
});

// Replacing the default texts: numeric value is replaced with a label.
var slider2 = node.widgets.append('Slider', root, {
    id: 'myslider2',
    min: 1,
    max 7,
    mainText: 'How do you feel?',        
    texts: {
        currentValue: function(widget, value) {
            let mood = [
                'Terrible!', 'Bad', 'Could be better',
                'Normal',
                'Not bad', 'Good', 'Great!'
            ];            
            return mood[(value-1)];
        }
    }
});


```

## Links

- [Source Code](https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/Slider.js)
- [List of Widgets](Widgets-v5)
