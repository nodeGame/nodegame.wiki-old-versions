 - status : in progress
 - version : 3.x

## Illustration

<figure>
  <img src="" alt="Illustration of the EndScreen widget">
</figure>

## Introduction

EndScreen is a widget that displays standard information for the end of a game, as well as several user forms, generally to be used in a *Widget Step*.

The email form and feedback form have client side data validation built in. 

## Main Options

- **headerMessage**: A string to display as the heading of the end screen. Default: `'Thank You for Participating!'`
- **message**: A string for the body of the end screen. Default: `'You have now completed this task and your data has been saved. Please go back to the Amazon Mechanical Turk web site and submit the HIT.'`

### Additional Options

- **showEmailForm**: A boolean that, when set to true, enables the email form. Default: `true`
- **showFeedbackForm**: A boolean that, when set to true, enables the feedback form. Default: `true`
- **showTotalWin**: A boolean that, when set to true, displays the total money won during the game. Default: `true`
- **showExitCode**: A boolean that, when set to true, displays the exit code received from the game. Default: `true`
- **maxFeedbackLength**: An integer that denotes the max character length the client should submit for feedback. This is used in the feedback input validation, to prevent excessive submissions, but should also be checked in the logic. Default: `800`

## Main methods

No public methods to call.

## Listeners

- **WIN**: Expects an object with the `total` and `end` attributes, denoting the total amount won and the end code, respectively.

## Events

When this widget is used and the email forms and feedback forms are enabled (on by default), the logic is expected to handle the corresponding events to save the input.

- **feedback**: Sends the user feedback as a string.
- **email**: Sends the user email as a string.

## Usecase

Example adapted from the StopGo game.

*Inside player.js:*
```js
// this uses the EndScreen in a Widget Step at the end of the game
stager.extendStep('end', {
    donebutton: false,
    frame: 'end.htm',
    widget: {
        name: 'EndScreen',
        root: "body",
        options: {
            title: false, // disable title for seamless Widget Step
        }
    }
});
```

*Inside logic.js:*
```js
stager.extendStep('end', {
    cb: function() {
        var code;
        var allMatchesInRound;
        var roles;
        var i;

        // this code uses the Matcher API
        allMatchesInRound = node.game.matcher.getMatches('ARRAY_ROLES_ID');

        // allMatchesInRound = node.game.matcher.getMatches();

        for (i = 0; i < allMatchesInRound.length; i++) {
            roles = allMatchesInRound[i];
            code = channel.registry.getClient(roles.RED);

            // send the WIN event to update the total wins and exit codes on the clients
            node.say('WIN', roles.RED, {
                total: node.game.totals[roles.RED],
                exit: code.ExitCode
            });

            code = channel.registry.getClient(roles.BLUE);

            node.say('WIN', roles.BLUE, {
                total: node.game.totals[roles.BLUE],
                exit: code.ExitCode
            });
        }

        // expecting email
        node.on.data('email', function(msg) {
            var id, code;
            id = msg.from;

            code = channel.registry.getClient(id);
            if (!code) {
                console.log('ERROR: no codewen in endgame:', id);
                return;
            }

            // Write email to CSV -- this function is not included in nodeGame
            appendToCSVFile(msg.data, code, 'email');
        });

        // expecting feedback
        node.on.data('feedback', function(msg) {
            var id, code;
            id = msg.from;

            code = channel.registry.getClient(id);
            if (!code) {
                console.log('ERROR: no codewen in endgame:', id);
                return;
            }

            // Write feedback to CSV -- this function is not included in nodeGame
            appendToCSVFile(msg.data, code, 'feedback');
        });

        // this is not included in the example; in the StopGo game, this saves the standard data.
        node.on.data('done', function(msg) {
            saveAll();
        });
    }
});

// suggested function to save the data
function appendToCSVFile(email, code, fileName) {
    var row, gameDir;

    gameDir = channel.getGameDir();
    row  = '"' + (code.id || code.AccessCode || 'NA') + '", "' +
        (code.workerId || 'NA') + '", "' + email + '"\n';

    fs.appendFile(gameDir + 'data/' + fileName + '.csv', row,
                  function(err) {
        if (err) {
            console.log(err);
            console.log(row);
        }
    });
}
```

## Links

- [List of Widgets](Widgets-API-v3)
