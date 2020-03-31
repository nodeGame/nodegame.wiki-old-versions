- status: complete
- version: 4.x
- follows from: [Step Callbacks](Step-Callbacks-Functions-v4)


Here we show a few full examples about how to exchange data between
players and server. As explained in the
[client types page](Client-Types-v4), the client-side code should go
in file `player.js`, and the server-side code should go in file
`logic.js`.


The use case is the following:

- a player makes a decision by typing a numeric value into an input
form 
- the value is validated, and sent to the server
- the server manipulates the value, and stores it to compute the final
  payoff of the player
- the server sends the final payoffs to all players


### Example 1: General exchange of data between player and server

**Client-Side (player.js).**

1. Get the input from user and validate it.

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

2. Send the input to the server.

```javascript
// Send the value to the server.
node.say('USER_INPUT', 'SERVER', parsedValue);
```

**Server-Side (logic.js).**

3. Declare an event-listener on incoming data "USER_INPUT", and save
the data into the channel's registry.

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

### Example 2: Submit data at the end of a step

Let us assume now that after the user typed its decision into the
input form, the step ends (no more actions in the same step). You can
then submit the user's value together with the end-of-step command:
`node.done`. As compared to Example 1., this solution has the
advantage of storing the data in the memory database with timestamps
and durations, and, moreover, it makes it easier to save all results
at the end of the game (see point 6).

Replace step 2. of Example 1. with:

```javascript.
node.done({ USER_INPUT: value});
```

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

### Example 3: Multiple submit of data

If a player must make take multiple decision in the same step (e.g. in
a real-time game), and all decision must be recorded and saved into
the results, then `node.set` should be used.

Replace step 2. of Example 2. with:

```javascript.
node.set({ USER_INPUT: value});
```

## Next Topics

* [Match players together and assign them roles](Matching-Roles-Partners-v4) 
