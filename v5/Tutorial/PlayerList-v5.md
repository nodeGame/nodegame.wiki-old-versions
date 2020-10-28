- status: complete
- version: 5.x
- follows from: [Step Callback Functions](Step-Callback-Functions-v5)

The player list is a special object containing the list of all the
clients _currently connected_ to a game room. It is accessible as a property of the `node.game` object.

```javascript
node.game.playerList;
node.game.pl; // Shorthand alias for playerList.
```

### Player vs Logic

For security reason, by default only the logic [client
type](Client-Types-v5) receive player list updates, while player client types have an empty player list.

To change this behavior edit the `notify` property inside the [channel configuration file](Channel-Configuration-v5).

### Player List updates

The player list is dynamic in size: when a player disconnects it gets removed from the player list and it gets added again upon reconnection.

```javascript
node.game.pl.size(); // 5 players.
// One player disconnects.
node.game.pl.size(); // 4 players.
// Player reconnects.
node.game.pl.size(); // 5 players again.
```

#### Listeners

When a new player connects (or reconnects) the following listeners are fired:

```javascript
// New connection.
node.on.pconnect(function(player) {
    console.log('A new player connected to the game: ' + p.id);
});
// Reconnection.
node.on.preconnect(function(player) {
    console.log('A previously disconnected player reconnected to the game: ' + p.id);
});
```

##### Logic initial value

The player list of the logic is pre-filled with the all players dispatched by the waiting room. This means that for this initial group of players, the listener _pconnect_ is not fired.

### Accessing the player objects

#### Get the ids of all players

You can get an array of all ids using querying the `id` index.

```js
var ids = node.game.pl.id.getAllKeys();
// [ 'id1', 'id2', 'id3' ]
```

#### Loop through all players

It is also possible to cycle through all connected players in a game,
for example to send a message to every player.

```javascript
node.game.pl.each(function(player) {
    node.say('hello!', player.id);
});
```

#### First and last

In a two-player game, it can be handy to access the first and last player directly:

```javascript
var player1 = node.game.pl.first();
var player2 = node.game.pl.last();
// Get the ids of the players.
var id1 = player1.id;
var id2 = player2.id;
```

### Player Objects

The player list contains objects of type `Player`, which can be
accessed if the id is of the player is known.

```javascript
var player;
if (node.game.pl.exist('some-player-id') {
    player = node.game.pl.get('some-player-id');
    console.log(player);

    // Player {    
    //     id: "some-player-id",
    //     sid: "ZmjmT_A6VQhTpY2CAAAA",
    //     clientType: null,
    //     stage: { stage: 1, step: 2, round: 1 },
    //     stageLevel: 50,
    //     stateLevel: 1,
    //     admin: false,
    //     clientType: 'player',
    //     count: 3,
    //     lang: {
    //         name: "English",
    //         shortName: "en",
    //         nativeName: "English",
    //         path: "en/"
    //     },
    //     disconnected: false,
    //     // Properties reserved for future use.
    //     group: null,
    //     ip: null,
    //     name: null,
    //     role: null,
    //     __proto__: Player
    // };
}
```

### NDDB

The player list object is an instance of the
[NDDB](https://github.com/nodeGame/NDDB) database, so all the NDDB
methods are available to the player list ast well.

## Next

* [Memory Database](Memory-Database-v5)

## Sources

* [PlayerList.js](https://github.com/nodeGame/nodegame-client/blob/master/lib/core/PlayerList.js)
* [NDDB](https://github.com/nodeGame/NDDB)
