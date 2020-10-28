- status: complete
- version: 5.x
- follows from: [Levels](Levels-v5)


Automated players are created as new [client types](Client-Types-v5)
by adding a new file the `game/client_types/` folder.

They can be created from scratch, or they can extend an existing one,
like in the example below.

```javascript
module.exports = function(treatmentName, settings, stager, setup, gameRoom) {

    var node = gameRoom.node;
    var ngc =  require('nodegame-client');
    var game, stager;

    // Get a copy of the 'player' client type and rename it.
    game = gameRoom.getClientType('player');
    game.nodename = 'autoplay';

    // Create a new stager object and start modifying it.
    stager = ngc.getStager(game.plot);

    // Steps can be extended individually, but it is often easier
    // to extend them all at the same time.
    stager.extendAllSteps(function(o) {

        // Store a reference to previous step callback.
        o._cb = o.cb;
        // Create a new step callback.
        o.cb = function() {
            var _cb, stepObj, id;

            // Get the previous step callback and execute it.
            stepObj = this.getCurrentStepObj();
            _cb = stepObj._cb;
            _cb.call(this);

            // Performs automatic play depending on the step.
            id = stepObj.id

            if (id === 'xxx') {
                // Do something.
            }
            else if (id === 'yyy') {
               // Do something else.
            }
            else if (id === 'zzz') {
               // Do yet another automated thing.
            }

            // Call node.done after a random time interval.            
            node.timer.randomDone();
        };

        // Return the extended step.
        return o;
    });

    game.plot = stager.getState();

    return game;
};
```

Furthermore, in the step callback of the automated player, it is
possible to simulate game events, such as disconnections and timeups.

```javascript
    // Instead of calling node.timer.randomDone()

    // Simulate a disconnection and reconnection
    // (authentication must be enabled in the channel to work).
    if (Math.random() < 0.5) {
        node.socket.disconnect();
        node.game.stop();
        node.timer.randomExec(function() {
            node.socket.reconnect();
        }, 4000);
    }
    else {
        // Simulate a timeup.
        if (Math.random() < 0.5) {
            node.timer.randomDone(1500);
         }
    }
```

## Launching an automated player

Automated players can be launched from the monitor interface, via the
[server API](Server-API-v5), or by specifying the client type as a
query parameter in the uri in the address bar of the browser, e.g:

```
http://localhost:8080/?clientType=autoplay
```

## Sources

* Writing an autoplay player when roles are defined, see the [Ultimatum Game](https://github.com/nodeGame/ultimatum/blob/roles/game/client_types/autoplay.js)


## Next

* Next: [Disconnections and Reconnections](Disconnections-and-Reconnections-v5)
