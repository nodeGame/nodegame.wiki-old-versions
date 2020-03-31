- status: complete
- version: 4.x
- follows from: [Step Callback Functions](Step-Callback-Functions-v4)


The player-list is a special object containing the list of clients
currently connected in a game room. It is accessible as a property of
the `node.game` object.

```javascript
node.game.playerList;
node.game.pl; // Shortcut to playerList.
```

When a player disconnects it is removed from the player-list.

```javascript
node.game.pl.size(); // 5 players.
// One player disconnects.
node.game.pl.size(); // 4 players.
```

The player-list contains objects of type `Player`, which can be
accessed if the id is of the player is known.

```javascript
var player;
if (node.game.pl.exist('ID') {
    player = node.game.pl.get('ID');
    console.log(player);
    
    // Player {    
    //     id: "ID",
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
```


It is also possible to cycle through all connected players in a game,
for example to send each a message.

```javascript
node.game.pl.each(function(player) {
    node.say('hello!', player.id);
});
```

_Important_: for security reason, normally only the logic
[client types](Client-Types-v4) receive player-list updates. So, the
player-list object in player client types is usually empty. To change
this behavior edit the `notify` property inside the
[channel configuration file](Channel-Configuration-v4).

## Next

* [Memory Database](Memory-Database-v4)

## Sources

* [PlayerList.js](https://github.com/nodeGame/nodegame-client/blob/master/lib/core/PlayerList.js)
* [NDDB](https://github.com/nodeGame/NDDB)
