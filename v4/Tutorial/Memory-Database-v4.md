- status: complete
- version: 4.x
- follows from: [PlayerList](PlayerList-v4)

The memory database is available as a property if the `node.game`
object.

```javascript
node.game.memory;
```

By default, all `node.set` messages and all the parameters passed to
`node.done` at the end of a step are saved in the memory database of
the logic [client type](Client-Types-v4) of the room.


```javascript
// Client A (stage: 1.1.1).
node.set({ foo: "bar" });
// Logic.
node.game.memory.size(); // 1

// Client B. (stage: 1.1.2).
node.done({ foo: "bar1"}, { foo1: "bar2" });
// Logic.
node.game.memory.size(); // 3 -> 1 item from A, 2 items from B.
```

There are two predefined sub-databases inside the memory database:
`player` and `stage`.

```javascript
// Items indexed by player.
node.game.memory.player.A.size(); // 1
node.game.memory.player.B.size(); // 2

// Items indexed by the stage in which they have been created.
node.game.memory.stage['1.1.1'].size(); // 1
node.game.memory.stage['1.1.2'].size(); // 2
node.game.memory.stage[new GameStage('1')].size(); // 1
node.game.memory.stage[node.game.getPreviousStep()).size(); // 1;
```

It is also possible to perform custom searches in the database.

```javascript
node.game.memory
        .select('foo', '=', 'bar')
        .and('player', '=', 'A')
        .fetch();
        
        // [ {
        //    foo: "bar",
        //    player: "A",
        //    stage: { stage: 1, step: 1, round : 1 },
        //    timestamp: 1476113729710
        // } ]
```

However, it often more efficient to define a new hash table in the
init function of the game:

```javascript
// In the init function.
node.game.memory.hash('fooa', function(item) {
    if (item.player === 'A' && item.foo ==='bar') {
        return item;
    }
});

// During the game.
node.game.memory.fooa.size(); // 1
```


Finally, the memory database is an instance of the
[NDDB](https://github.com/nodeGame/NDDB) database, and inherits all
its methods.

    
## Next

* [Learn when and how to define event listeners](Event-Listeners-v4)

## Sources

* [GameDB](https://github.com/nodeGame/nodegame-client/blob/master/lib/core/GameDB.js)
* [NDDB](https://github.com/nodeGame/NDDB)
