- status: complete
- version: 5.x
- follows from: [Memory Database](Memory-Database-v5)

The `game.setup` file is the place for the settings that are not directly related to the game, but rather to the nodeGame engine.

The setup file is loaded _once_ and shared with all game rooms in the channel. This makes it the proper place to load a store large collection of items (for instance images to be classified).

```javascript

const path = require('path');
const NDDB = require('NDDB').NDDB;

module.exports = function(settings, stages, dir, level) {

    var setup = {};

    // General settings.
    setup.debug = true;
    setup.verbosity = 1;

    // Default Windows settings.
    setup.window = {
        promptOnleave: !setup.debug
    };

    // Loading an external database shared with all game rooms.
    setup.db = new NDDB({ update: { indexes: true }});
    setup.db.loadSync(path.join(dir, 'private', 'articles-redux-1.json'));
    console.log('DB SIZE: ' + setup.db.size());

    return setup;
};
```


## Next Topics

* Next: [Learn when and how to define event listeners](Event-Listeners-v5)
