- status: complete
- version: 5.x
- follows from: [Push Manager](Push-Clients-v5)

## Overview

nodeGame serves all the static files for the browser from the
`public/` folder of a a game directory. 

However, it is possible to dynamically create HTML pages by adding
 files inside the `views/` folder.

The `views/` folder is further divided in two sub-folders:
`templates/` and `/contexts/`.

The `templates/` folder contains [Jade](http://jade-lang.com/) files
to create and format HTML pages. Templates support dynamic code, and
allow to reuse and extends components across pages.

The `/contexts/` folder contains JavaScript files exporting a local
context function for a template file. The context must include all the
variables that will be used to fill-in the content of a template.

### Template files

For instructions how to format Jade templates refer to the Jade
[language reference](http://jade-lang.com/reference/).

### Context Functions

Context files are contained inside the `views/contexts/` folder. They
must be named after one of the template files inside
`views/templates/`. For example, if a template is named
'instructions.jade', the context must be named 'instructions.js'.

Each context file must export a function that returns the context,
that is an object containing all the variables that the template will
need to fill-in its content.

Each context-function receives two input parameters: the current
game-settings, and the headers of the connections of the requester.

The input parameters can be used inside the context-function to
perform operations such as: randomization, shuffling, customization,
as in the example below.

```javascript

var J = require('JSUS').JSUS;

module.exports = function(settings, headers) {

    var coins = settings.standard.COINS;
    var values = [
        Math.floor(coins/2, 10),
        coins,
        0
    ];

    return {
        "x": 0,
        "title": "Quiz",
        "beforeStarting": "Before starting the game answer the following questions:",
        "Q": "Q",
        "howManyCoins": "How many coins will you divide with your partner?",
        "vals": J.shuffle(values),     
        "correctAnswers": "Correct Answers:"
    };
};
```

## Templating rules

When a client requests the uri:

`http://myserver/mygame/mypage.html` 

the following operations are performed:

1. First, the page is looked up inside the `public/` folder, and if
found it is served immediately.

2. Then, the page is looked up inside the `views/templates/` folder.

3. Together with the template, the related context files will be
located in `views/contexts/`. If found, the context will be used to
build the template.

4. The template is rendered and sent to the client.

5. If no static file in `public/` and no template in
`views/templates/` was found, thena 404 Page Not Found code is sent to
the client.


## Internationalization

Using templates and contexts permits to achieve the
internationalization of a game easily.

To do so, create a new sub-directory for each language you want to
support inside the `views/contexts/` folder. For example
`views/contexts/en/`, and `views/contexts/de/`. In each of those
sub-directory you can store the context translated in the language of
choice.

When the address `http://myserver/mygame/LANG/mypage.html` is
requested, the template `views/templates/mypage.jade` will be rendered
using the context `views/templates/LANG/mypage.js`.

### Language Selector

Optionally, you can add file named `languageInfo.json` inside each of
the language-directories containing information about the language. At
the very least, the languageInfo file must contain the following data:

```javascript
{
    "name": "English",
    "nativeName": "English",
    "flag": ""
}
```

Such information will be collected and can be queried by connected
clients. For example, the [Language Selector](LanguageSelector-Widget)
widget automatically displays all the languages available for a game
and allows to select one from a list.
  
## Next Topics

Next: [Logging](Logging-v5)

## See Also

* [ultimatum game views/ folder](https://github.com/nodeGame/ultimatum/tree/master/views)
* [nodegame-server HTTP configuration file](https://github.com/nodeGame/nodegame-server/blob/master/conf/http.js)
* [nodegame-server Page-Manager file](https://github.com/nodeGame/nodegame-server/blob/master/lib/PageManager.js)



