 - status : complete
 - version : 4.x

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/end-screen-widget.jpeg" alt="Illustration of the EndScreen widget">
</figure>

## Introduction

EndScreen is a widget that displays standard information for the end of a game,
as well as several user forms. It is generally used as a *Widget Step*.

The email form and feedback form have client side data validation built in. 

## Main Options

- **showEmailForm**: A boolean that, when set to true, enables the email form. Default: `true`
- **showFeedbackForm**: A boolean that, when set to true, enables the feedback form. Default: `true`
- **showTotalWin**: A boolean that, when set to true, displays the total money won during the game. Default: `true`
- **showExitCode**: A boolean that, when set to true, displays the exit code received from the game. Default: `true`
- **totalWinCurrency**: The currency of the earnings. Default: 'USD'.
- **totalWinCb**: A callback that receives an object with information such as total earnings, and returns the value to display as total win.
- **email**: Options to be passed to the Email widget
- **feedback**: Options to be passed to the Feedback widget 

### Main texts options

- **headerMessage**: A string to display as the heading of the end screen. Default: `'Thank You for Participating!'`
- **message**: A string for the body of the end screen. Default: `'You have now completed this task and your data has been saved. Please go back to the Amazon Mechanical Turk web site and submit the HIT.'`

### Additional Options

## Main methods

No public methods to call.

## Listeners

- **WIN**: Expects an object with the `total` and `end` attributes, denoting the total amount won and the end code, respectively.

## Events

None.

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
            title: false, // Disable title for seamless Widget Step.
            panel: false, // No border around.
            showEmailForm: true,
            showFeedbackForm: true,
            email: {
                texts: {
                    label: 'Enter your email (optional):',
                    errString: 'Please enter a valid email and retry'
                }
            },
            feedback: { minLength: 50 }             
        }
    }
});
```

*Inside logic.js:*

```js
stager.extendStep('end', {
    cb: function() {

        // Send message to each player that will be caught
        // by EndScren widget, formatted and  displayed.
        gameRoom.computeBonus({

            // The names of the columns in the dump file.
            // Default: [ 'id' 'access', 'exit', 'bonus' ]
            header: [ 'id', 'type', 'workerid', 'hitid',
                       'assignmentid', 'exit', 'bonus' ],

            // The name of the keys in the registry object from which
            // the values for the dump file are taken. If a custom
            // header is provided, then it is equal to header.
            // Default: [ 'id' 'AccessCode', 'ExitCode', winProperty ] || header
            headerKeys: [ 'id', 'clientType', 'WorkerId',
                          'HITId', 'AssignmentId', 'ExitCode', 'win' ],

            // If different from 1, the bonus is multiplied by the exchange
            // rate, and a new property named (winProperty+'Raw') is added.
            // Default: (settings.EXCHANGE_RATE || 1)
            exchangeRate: 1,
            
            // The name of the property holding the bonus.            
            // Default: 'win'
            winProperty: 'win',

            // The decimals included in the bonus (-1 = no rounding)
            // Default: 2
            winDecimals: 2,

            // If a property is missing, this value is used instead.
            // Default: 'NA'
            missing: 'NA'

            // If set, this callback can manipulate the bonus object
            // before sending it.
            // Default: undefined
            cb: function(o) { o.win = o.win + 1 },
            
            // If TRUE, sends the computed bonus to each client
            // Default: true
            say: true,
            
            // If TRUE, writes a 'bonus.csv' file.
            // Default: true
            dump: true,

            // If TRUE, console.log each bonus object
            // Default: false
            print: true
        });

        // Do something with eventual incoming data from EndScreen.
        node.on.data('email', function(msg) {           
           // Store msg to file.           
        });
        node.on.data('feedback', function(msg) {
           // Store msg to file.
        });
    }
});
```

## Links

- [List of Widgets](Widgets-v4)
