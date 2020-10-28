- status: complete
- version: 5.x
- follows from:  [Authorization Rules](Authorization-Rules-v5)

## Overview

Levels break the game in independent parts, each of which can be played with different synchronization rules and group sizes.

The typical use case consists in having an initial single-player level where the game is explained, then the client reaches the waiting room of the next level where it waits to start the multiplayer game.

## Create a New Level

To create a new game level, create a new folder named after the level
inside the `levels/` directory. For instance to create a level named `part2` `levels/part2/`.

Then, inside the new level directory, add a `game/` folder, and optionally a `waitroom/` folder, following exactly the same structure as for the corresponding folders at the top-level directory.

## Levels Order

The order of the levels must be specified manually. The game begins by running the code inside the `game/` folder; this is the default level. Then, clients must be moved to the next level manually.

### Code Example to move clients to a levels

A client finishing the default level could be moved to a next level named `part2` with the following code.

The client sends a message to the logic:

```javascript
node.say('level_done');
```

Upon receiving it, the logic uses the [Server API](Server-API-v5)
to move the client to next room (usually the waiting room of the next
level).

```javascript
node.on.data('level_done', function(msg) {
    var levelName = 'part2';
    var roomName = 'part2WaitRoom';
    // Move client to the next level.
    // (async so that it finishes all current step operations).
    setTimeout(function() {
        console.log('moving client to next level: ', msg.from);
        channel.moveClientToGameLevel(msg.from, levelName, roomName);
    }, 100);
});
```

## Settings and Treatments

Levels in the `levels/` folder inherit all the settings and treatments from the default level (inside the top-level `game/` folder). In addition, they can also add new settings and treatments or extend existing ones by specifying an own `game.settings.js` inside the level's `game/` directory.

## Next

* [Automated Players](Automated-Players-v5)
