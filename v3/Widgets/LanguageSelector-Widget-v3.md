 - status : complete
 - version : 3.x

## Illustration
<figure>
  <img src="http://nodegame.org/images/wiki/LanguageSelector.png" alt="Illustration of the LanguageSelector widget.">
  <br>
  <figcaption>Appearance of the LanguageSelector widget when two
  languages are available.</figcaption>
</figure>


## Introduction

The language selector widget allows to select the language for a game
from a list of available languages retrieved from the server.

The result of the selection are saved in `node.player.lang`. The
language object contains at least the following properties:

- `name`: Name of the language in English.
- `nativeName`: Native name of the language.
- `shortName`: An abbreviation for the language, e.g. de, it, es, etc.


## Main Options

- **usingButtons**: if TRUE, the interface displays buttons, otherwise
    a drop-down selector is used. Default: TRUE.
    
## Main Methods

- **setLanguage**: Sets the chosen language in the widget and also
    calls `node.setLanguage`, which in turn emits 'LANGUAGE_SET' if
    selection is valid.

## Listeners

**in.say.LANG**: updates the list of available languages.

## Usecase

```js
// Creates and append a new MoneyTalks widget.
var root = document.body;
var moneyTalks = node.widgets.append('LanguageSelector', root);
```

## Links

- [List of Widgets](Widgets-v3)
