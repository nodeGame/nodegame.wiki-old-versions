- status: complete
- version: 5.x
- follows from: [Step Callbacks](Step-Callbacks-Functions-v5)

## Overview

There are 4 main functions to send data:

| Command  | Descr   | Recipient code required  | 
|---|---|---|
| node.say  | sends data to other clients or server   | yes, must implement `node.on.data` event listener  | 
| node.set  | sends data to server  | no, automatically inserted in `node.game.memory` database   | 
| node.done | terminates current step, and calls node.set  | see `node.set`   | 
| node.get  | invokes a remote function, and waits for return value | yes, must implement `node.on` event listener |

and 1 function to handle receiving data:

| Command  | Descr | 
|---|---|
| node.on.data  | catches `node.say` and `node.set` messages with given label  | 
| node.on  | catches internal events, as well as requests from `node.get` |

Below, you can find some examples about how to use the commands to exchange
data.  Imagine the following common use case:

- a player makes a decision by typing a numeric value into an input form,
- the value is validated, and sent to the server,
- the server manipulates the value and stores it to compute the final payoff of
  the player,
- the server sends the final payoffs to all players.

Note! As explained in the [client types page](Client-Types-v5), the client-side
code goes in file `player.js`, and the server-side code goes in file `logic.js`.

### Example 1: node.say and node.on.data

**Client-Side (player.js).**

1. Get the input from user, validate it, and show error if not valid.

```javascript
// Assuming a user typed a numeric value into an input form.
var value = W.getElementById('myFormId').value;
// Parse the input and validate its range, say 0-10.
var parsedValue = JSUS.isInt(value, 0, 10); // See also JSUS.isNumber.
if (false === parsedValue) {
    alert('Invalid value: ' + value);
    return;
}
```

2. Send the validated input to the server using `node.say`.

```javascript
// Send the value to the server.
node.say('USER_INPUT', 'SERVER', parsedValue);
```

#### Explanation 

`node.say` takes the following input parameters: 

- a label (`USER_INPUT`), 
- a recipient (`SERVER`), 
- an optional data value (`parsedValue`).

**Server-Side (logic.js).**

3. Declare an event-listener on incoming data labeled "USER_INPUT" and save it
   into the channel's registry.

```javascript

function storeData(msg) {
    var parsedValue = msg.data;
    // Important! Re-validate the incoming data if sensitive (for example,
    // it is used to compute payoffs).
    // Manipulate the input (e.g. exchange from input to a monetary value).
    parsedValue *= 1.5;
    // Store the value in the registry under the ID of the player (msg.from).
    var userData = channel.registry.getClient(msg.from);
    // Update/create the win variable.
    userData.win = userData.win ? (userData.win + parsedValue) : parsedValue;
}

node.on.data('USER_INPUT', storeData);
```

4. At the end of the game, you want to notify all connected players of
their total win.

```javascript
// Loop through all connected players.
node.game.pl.each(function(player) {
    // Get the value saved in the registry, and send it.
    var cumulativeWin = channel.registry.getClient(player.id).win;
    node.say('WIN', player.id, cumulativeWin);
});
```

**Client-Side (player.js).**

5. Display the total amount won by the user.

```javascript
// Listen to the WIN message.
node.on.data('WIN', function(msg) {
    // Write inside the element with ID 'win' the amount of money won.
    W.setInnerHTML('win', 'You won: ' + msg.data);
});
```


### Example 2: node.set

If a player takes multiple decision in the same step (e.g., in a real-time
game), `node.set` can be used to record all of these.

Replace step 2. of Example 2. with:

```javascript
node.set({ USER_INPUT: value});
```

#### Explanation

`node.set` takes the following input parameters:

- the value to set, it can be an object or a string; if a string, it will be
  transformed into an object containing a key with the name of the string set to
  true (`{ USER_INPUT: value}`),
- the recipient (default `SERVER`),
- (Optional) The text property of the message (if set, you can define
  `node.on.data` listeners).

Important! The content of each set message is added to the database, there is no
check for duplicates.


### Example 3: node.done

Let us assume now that after the user typed its decision into the input form,
the step ends (no more actions in the same step). You can then submit the user's
value together with the end-of-step command: `node.done`. As compared to Example
1., this solution has the advantage of storing the data in the memory database
with timestamps and duration; moreover, it makes it easier to save all results
at the end of the game (see point 6 below).



**Client-Side (player.js).**

Replace step 2. of Example 1. with:

```javascript
node.done({ USER_INPUT: value});
```

#### Explanation

`node.done` takes a variable number of input parameters, and each
of them is considered as an independent call to `node.set`.

**Server-Side (logic.js).**

Replace step 3. of Example 1. with:

```javascript
node.game.memory.on('insert', function(item) {
    if ('string' === typeof item.USER_INPUT) {
        // Save data to registry, the same as in Example 1. 
        storeData({ data: item.USER_INPUT });
    }
});
```

6. Saves all results in the `data/` folder of the game.

```javascript
node.game.memory.save('myresults.json');
```

### Example 4: node.get

To invoke a specific function on a remote client and to asynchronously receive
back the results of the call, use `node.get`. The example below executes
server-side validation of the input value.

**Client-Side (player.js).**

```javascript
node.get('validateInput', function(data) {
    if (data.valid) node.done({USER_INPUT: parsedValue });
},
'SERVER',
{
    data: parsedValue 
});
```


#### Explanation:

`node.get` gets the following input parameters:

- The label of the GET message ('validateInput'),
- The callback function that handles the return message,
- (Optional) The recipient of the msg (default: 'SERVER'),
- (Optional) An object with extra options:
  - timeout: The number of milliseconds after which the return 
             listener will be removed,
  - timeoutCb:  A callback function to call on timeout (no reply received),
  - executeOnce: TRUE if listener should be removed after one execution,
  - data: The data field of the GET msg,
  - target Override the default DATA target of the GET msg.

**Server-Side (logic.js).**

```javascript
node.on('get.validateInput', function(msg) {
    // Validate data.
    var valid = true;
    if (!J.isInt(msg.data)) valid = false;
    return { valid : valid };
});
```


## Next Topics

* [Match players together and assign them roles](Matching-Roles-Partners-v5) 
