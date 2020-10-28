- status: complete
- version: 5.x
- follows from: [PlayerList](PlayerList-v5)

The memory database is available under `node.game.memory` of
the logic [client type](Client-Types-v5).

### Adding Items to the Memory Database

The database contains all the input parameters of `node.done`, which terminates the step.

```js
// Client id=A (stage: 1.1.1).
node.done({ foo: "bar"});
// Client id=B (stage: 1.1.1).
node.done({ foo: "bar"});

// Logic.
node.game.memory.size(); // 2 -> 1 item from A, 1 item from B.
```

Clients can automatically save data in the memory database, using the command `node.set`, without terminating the step. This is specially useful for continuous-time games.

```js
// Client id=A (stage: 2.1.1).
node.set({ value: 1 });
node.set({ value: 2 });
// Logic.
node.game.memory.size(); // 4 (2 items from previous example, and 2 new ones).
```

Finally, the logic can manually add items to the database. Each item must contain a `player` and `stage` property, or an error will be raised. This is useful to fill in values from disconnected players.

```js
// Logic.
node.game.memory.add({
    player: 'B',
    stage: { stage 2, step: 1, round 1}
    value: 3
});
node.game.memory.size(); // 5 (4 items from previous examples, and 1 new one).
```

### Querying the Memory Database

Here are a few examples of database queries:

```javascript
// All items that contain a property called 'value'.
node.game.memory.select('value').fetch();

// [
//     {
//         value: 1,
//         player: "A",
//         stage: { stage: 2, step: 1, round : 1 },
//         timestamp: 1476113729710
//     },
//     {
//         value: 2,
//         player: "A",
//         stage: { stage: 2, step: 1, round : 1 },
//         timestamp: 166146733111
//     },
//     {
//         value: 3,
//         player: "B",
//         stage: { stage: 2, step: 1, round : 1 },
//         timestamp: 176547899812
//     },
// ]

// All items that contain a property named 'foo' equal to 'bar' from player 'A'.
node.game.memory
    .select('foo', '=', 'bar')
    .and('player', '=', 'A')
    .fetch();

// [ {
//     foo: "bar",
//     player: "A",
//     stage: { stage: 1, step: 1, round : 1 },
//     timestamp: 1476113729710
// } ]


// All items with a property 'value' between 0 and 5.
node.game.memory
    .select('value', '>', 0)
    .or('value', '<', 5)
    // Consider also the '<>' operator for the same result.
    .fetch();

// [
//     {
//         value: 1,
//         player: "A",
//         stage: { stage: 2, step: 1, round : 1 },
//         timestamp: 1476113729710
//     },
//     {
//         value: 2,
//         player: "A",
//         stage: { stage: 2, step: 1, round : 1 },
//         timestamp: 166146733111
//     },
//     {
//         value: 3,
//         player: "B",
//         stage: { stage: 2, step: 1, round : 1 },
//         timestamp: 176547899812
//     },
// ]

```

### Hash-Databases

A hash function takes an item from the database an returns an alphanumeric id, that is a "hash". Based on this hash, the memory databases constructs sub-databases for easy access to coherent collections of items.

There are two predefined sub-databases inside the memory database:
- `node.game.memory.player`: hashes items by player id;
- `node.game.memory.stage`: hashes items the stage of creation.

Here are a few examples about how to use those hash databases:

```js
// Items indexed by the player id.
node.game.memory.player.A.size(); // 3
node.game.memory.player.B.size(); // 2

// Get all items from previous step.
node.game.memory.stage[node.game.getPreviousStep()].size(); // 2;

// Get items indexed from given stages.
node.game.memory.stage['1.1.1'].size(); // 2
node.game.memory.stage['2.1.1'].size(); // 3

// This is equivalent to using a GameStage object
// (first import the GameStage class).
const ngc = require('nodegame-client');
const GameStage = ngc.GameStage;
node.game.memory.stage[new GameStage('1.1.1')].size(); // 2
```


#### Defining Custom Hash Databases

It is often convenient and more efficient to define a custom hash database.

```js
// In the init function of logic.

// A hash function containing all items from player A that have a property
// 'foo' with value 'bar'.
node.game.memory.hash('bar_A', function(item) {
    if (item.player === 'A' && item.foo ==='bar') {
        return item;
    }
});

node.game.memory.bar_A.size(); // 1
```

### Saving Memory Database to a File

You can save the content of the memory (or any subset of it) with:

```js
// Save all items.
node.game.memory.save('data.json');

// Save data from stage 1.
node.game.memory.stage['1.1.1'].save('data_stage1.json');

// Save data from player 'A'
node.game.memory.player.A.save('data_player_A.json');

// Save the result of a query.
node.game.memory
    .select('value', '>', 0)
    .or('value', '<', 5)
    .save('data_values_05.json');
```

The data will be saved under `data/roomXXX/`, where XXX is the room number.

#### CSV Files

If you prefer to save a CSV (comma separated value) file, you can simply give
the extension `.csv`.

```js
node.game.memory.save('data.csv');
```

If your data contain objects, their first-level properties are automatically expanded and written under a new column named `topPropertyName.nestedPropertyName`. For instance, assuming you have just one object in the memory database:

```js
// Manually add an item containing objects.
node.game.memory.add({
    player: 'A',
    stage: { stage: 1, step: 1, round: 1 },
    data: "A",
    nestedData: { decision: "GO", time: 1234 }
});
```
and you save it to a csv file:

```js
node.game.memory.save('data.csv');
```
you will get:

```csv
"player","stage.stage","stage.step","stage.round","data","nestedData.decision","nestedData.time","timestamp"
"A",1,1,1,"A","GO",1234,1587540763230
```

##### Deeper Nested Objects

However, if your database has objects nested at deeper levels, such in the
example below:

```js
// Adding an item with an object nested in an object.
node.game.memory.add({
    player: 'A',
    stage: { stage: 1, step: 1, round: 1 },
    data: "A",
    nestedData: { decision: "GO", time: 1234 },
    doubleNestedData: {
        level1var: "level1",
        level1: {
            level2var: "level2",
            level2: {
                level3: "Hello World"
            }
        },
    }
});
```

and you save it to a csv file:

```js
node.game.memory.save('data.csv');
```

you will get an `[Object Object]` reference:

```csv
"player","stage.stage","stage.step","stage.round","data","nestedData.decision","nestedData.time","doubleNestedData.level1var","doubleNestedData.level1","timestamp"
"A",1,1,1,"A","GO",1234,"level1","[Object Object]",1587540763230
```

That is, the object nested inside `doubleNestedData.level1` is not automatically expanded for performance reasons. You will need to specify an _adapter_ as option to the save method.

The adapter is an object whose properties are functions returning a value to save under a column named after the property name. For instance:

```js
// Saving the database with an adapter;
node.game.memory.save('data.csv', {
    adapter: {
        'doubleNestedData.level1': function(item) {
            // Return the value of interest from the double nested structure.
            return item.doubleNestedData.level1.level2.level3;
        }
    }
});
```

will generate:

```csv
"player","stage.stage","stage.step","stage.round","data","nestedData.decision","nestedData.time","doubleNestedData.level1var","doubleNestedData.level1","timestamp"
"A",1,1,1,"A","GO",1234,"level1","Hello World",1587540763230
```

Notice that adapter function extracted only one variable from the double nested object. If you wish to extract more values, simply add a new property to the adapter.

Usually, only some specific items in the database will have a deep nested structure such as in the example above. So, you need to make sure that the adapter function can handle any item in the database.

```js
// Adding another object without the double nested structure.
node.game.memory.add({
    player: 'A',
    stage: { stage: 2, step: 1, round: 1 },
    data: 'B'
})

// Improving the adapter to handle any item.
node.game.memory.save('data.csv', {
    adapter: {
        'doubleNestedData.level1': function(item) {
            // In case the item does not contain a double nested structure.
            if (!item.doubleNestedData) return 'NA';
            // Return the value of interest from the double nested structure.
            return item.doubleNestedData.level1.level2.level3;
        }
    }
});
```

And you will get:

```csv
"player","stage.stage","stage.step","stage.round","data","nestedData.decision","nestedData.time","doubleNestedData.level1var","doubleNestedData.level1","timestamp"
"A",1,1,1,"A","GO",1234,"level1","Hello World",1587540763230
"A",2,1,1,"B","NA","NA","NA","NA",1587540763231
```

### Learn more about the Memory Database

Finally, the memory database is an instance of the
[NDDB](https://github.com/nodeGame/NDDB) database, and inherits all
its methods.


## Next

* [Learn about the Game Setup file ](Game-Setup-v5)

## Sources

* [GameDB](https://github.com/nodeGame/nodegame-client/blob/master/lib/core/GameDB.js)
* [NDDB](https://github.com/nodeGame/NDDB)
