- status: complete
- version: 3.x
- follows from: [Handling Disconnections](Handling-Disconnections-v3)

## How to Enable the Push Manager

The push manager "pushes" clients that are lagging behind in the game
sequence to go to the next step.

To enable this feature, add the property `pushClients` to the steps
and stages that require this feature, like in the example below:

```javascript
// Enable pushClients with default settings.
stager.extendStep('step_with_default_push', {
    pushClients: true,
});
// Or with custom settings.
stager.extendStep('step_with_custom_push', {
    pushClients: {
        offset: 20000, // Default: 5000
        reply:   5000, // Default: 2000
        check:   2000 // Default: 2000
});
```

Also make sure that those steps have a timer property (either in the
[TIMER](Game-Basics-v3) object in `game.settings.js` or directly in
the steps).

### How does it work

1. When the step timer expires, if there are still clients that have
not yet finished the current step (stageLevel equals to DONE), then the
push manager will start a timer of duration `offsetWaitTime`.

2. At the end of this timer, clients that are not yet DONE (or that
have not stepped forward) will be pinged.

3. Upon receiving the ping, the client will try to call the step
timeup function, and then try to step forward.

4. The reply to the ping must arrive to the logic before
`replyWaitTime` milliseconds, otherwise the client will be
disconnected. 

5. If a reply is received in time, then another timer is started of
duration `checkPushWaitTime`, after which it will be checked if the
client has successfully stepped forward. If not, the client will be
disconnected.

## Next

* [Learn about how to dynamically create pages and views](Views-v3)

## Sources

* [PushManager Class](https://github.com/nodeGame/nodegame-client/blob/master/lib/core/PushManager.js)
* [Listener for Push Events](https://github.com/nodeGame/nodegame-client/blob/master/listeners/internal.js)
* [Listener for Timeup](https://github.com/nodeGame/nodegame-server/blob/master/lib/rooms/GameRoom.js)
  
## Go Back to 

* [Go Live](Go-Live-v3)
* [Home](Home)
