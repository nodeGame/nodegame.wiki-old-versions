 - status : complete
 - version : 5.x

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/v5/end-screen-widget.jpeg" alt="Illustration of the EndScreen widget">
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

## Listeners

- **WIN**: Expects an object with the `total` and `end` attributes, denoting the total amount won and the end code, respectively.

## Usecase

Example adapted from the [StopGo](https://github.com/nodeGame/stopgo)
game.

*Inside player.js:*

```js
// EndScreen in a Widget Step at the end of the game.
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

First, you need to keep track of earnings updating the `win` property
of each client. Clients are stored in the channel's registry, and
information is kept even after a disconnection.

```javascript
// Let us assume that the player '1234' got a payoff of 10 this round
var payoff = 10;
var playerId = '1234';
var client = channel.registry.getClient(playerId);
// For nodeGame <= 5.6.1 you need to check if the win property is defined.
if ('undefined' === typeof client.win) {
    client.win = payoff;
}
else {
    client.win += payoff;
}
```

At the end of the game, you can call the `computeBonus` method of the
`gameRoom` object. This method will send the current value of the
`win` property to the corresponding player and save a file with all
the values inside the data directory.

```js
stager.extendStep('end', {
    cb: function() {
        // All available options shown here.
        gameRoom.computeBonus({

            // The names of the columns in the dump file.
            //
            // The values for  each column are extracted from the
            // client objects in the registry, property names are
            // matched exactly. 
            // 
            // nodeGame 5.8.0+
            // Furthermore, it is possible to
            // specify a different property name by passing an array,
            // instead of a string, where the first element is the
            // name of the column in the dump file, and the second
            // element is the name of the property in the client object.
            // It is also possible to pass a function as second
            // element, and in this case the return value is written
            // to file.
            //
            // The default names included depend on other options.
            // Default: [ 'id' 'type', 'bonus' ].
            //
            // If `amt` option is true or AMT information is found,
            // the following fields are added:
            // [ 'workerid', 'hitid', 'assignmentid', 'access',
            //   'exit', 'approve', 'reject' ];
            //
            // If `addDisconnected` option is true, the following
            // fields are added:
            // [ 'disconnected', 'disconnectedStage' ].
            //
            header: [ 
                'id', [ 'type', 'clientType' ], 'win',
                [ 'approve', item => !item.disconnected ]
            ],

            // DEPRECATED 5.8.0+
            // The name of the keys in the registry object from which
            // the values for the dump file are taken.
            // Default: equal to header.
            //
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

            // If TRUE, it appends to an existing file (if found).
            // Default: false
            append: true,

            // An optional callback function to filter out clients
            // Default: undefined
            filter: function(client) {
                return true; // keeps the client (else skips it).
            },

            // An array of client objects to use instead of the clients
            // in the registry.
            // Default: undefined
            clients: [ client1, client2 ]

            // If TRUE, console.log each bonus object
            // Default: false
            print: true,

            // nodeGame 5.7.0+
            // If TRUE, currently diconnected players are added to the
            // printout and dump file.
            // Default: false
            addDisconnected: true,
            
            // nodeGame 5.8.0+
            // If TRUE, it forces writing AMT columns into dump file,
            // if FALSE, it prevents it.
            // Default: undefined.
            amt: true
            
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

- [Source Code](https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/EndScreen.js)
- [List of Widgets](Widgets-v5)
