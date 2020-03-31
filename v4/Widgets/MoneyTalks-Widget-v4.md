 - status : complete
 - version : 4.x

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/money-talks-widget.jpeg" alt="Illustration of the MoneyTalks widget">
</figure>

## Introduction

The MoneyTalks is a customizable button to display cumulative earnings
of a participant in a game.

## Main Options

- **currency**: A string containing the name of the currency for the
    earnings. Default: 'ECU' (Experimental Currency).
- **money**: Initial value of earnings.
- **precision**: How many digits after 0 to display. Default: 2.

### Additional Options

- **currencyClassname**: The name of the class for the span displaying
    the name of the currency. Default: 'moneytalkcurrency'.
- **moneyClassName**: The name of the class for the span displaying
    current earnings. Default: 'moneytalksmoney'.
  
## Main methods

**update(amount, clear)**: Adds the value of amount to current
  earnings and updates the display. Parameter `amount` may be a number
  or a string parsable into a number. If parameter `clear` is set to
  TRUE, then the current earnings are set equal to the value of
  `amount`.
**getValues**: returns the current value of earnings.

## Listeners

**MONEYTALKS**: calls the update method.

## Usecase

```js
// Creates and append a new MoneyTalks widget.
var root = document.body;
var moneyTalks = node.widgets.append('MoneyTalks', root, { 
    currency: 'USD',
    money: 10
});

// Add 10 units to earnings.
moneyTalks.update(10); // Current display is equal to 20.00.

// Sets the counter 5 units to earnings.
moneyTalks.update(5, true); // Current display is equal to 5.00.
```

## Links

- [List of Widgets](Widgets-v4)
