- status: complete
- version: 4.x
- follows from: [Synchronization](Synchronization-v4)

## Available Methods for Handling Disconnections

In every online experiment, deciding how to handle disconnections
(dropouts) is key. nodeGame offers a standard solution and a large
array of customization options.

<a name="channel"></a>
### Channel Reconnection Options

Reconnections can be enabled/disabled in file
`channel/channel.settings.js`, by changing the following options:

* **enabledReconnections**: If TRUE, reconnections are enabled
    (default TRUE).
    
* **sameStepReconnectionOnly**: If TRUE, reconnections are enabled
    only within the same game step (default: FALSE).
    
* **disposeFailedReconnections**: If TRUE, clients that tried to
    reconnect but failed are disposed. If FALSE, failed reconnections
    are treated as new connections (default FALSE).  A reconnection
    can fail for any reason, but in particular, if:
    - parameter `enabledReconnections` is FALSE.
    - parameter `sameStepReconnectionOnly` is TRUE, and the
    client's stage and the logic's stage are different.
    - the room in which the client was before disconnecting
      disconnection is no longer existing.
      
### Size-Handlers

A more precise way to handle dropouts is to setup a size-handler as a
[step property](https://github.com/nodeGame/nodegame/wiki/Client-Types-v4#step-properties). Size-handlers
are fired when a change in the size of the
[player-list](PlayerList-v4) takes place. There are three types of
size-handlers:

* **minPlayers**: checked whenever a player disconnects.

* **maxPlayers**: checked whenever a player connects or reconnects.

* **exactPlayers**: checked whenever player
  connects/disconnects/reconnects. _Important_: it cannot be defined
  if either `minPlayers` or `maxPlayers` is also set.

All the size-handlers follow the same syntax. Their value can be a
number (_threshold_), or an array containing up to three elements
ordered as follows:

1. _threshold_: the desired value of min/max/exact connected players.

2. _threshold\_cb_: a callback executed when the specified threshold
is passed.

3. _recovery\_cb_: a callback executed when the number of connected
players is correct again (viz. after _threshold\_cb_ was executed
once).

#### Default behavior when a size handler is set.

When a min|max|exact threshold is hit, the following steps are taken:

1. The game is paused (default: for 30 seconds).

2. A message is displayed informing all connected clients of the
situation.

3. If the correct number of players is not restored in time, the game
is resumed normally.

4. Otherwise, if a _threshold\_cb_ was defined, it is executed. Here,
the game developer could terminate the game, replace the player with a
bot or do nothing.

5. If the correct number of connected players is restored after
_threshold\_cb_ is executed once, then the _recovery\_cb_ is executed
(if one is defined).

It is possible to customize the default waiting time, and text
displayed while the game is paused in file `game/game.settings.js` by
defining  the following variables:

* **WAIT\_TIME**: The amount of seconds to wait for a player to
reconnect. Default 30.

* **WAIT\_TIME\_TEXT**: The text to display when on screen to the
    other players. 
   
<a name="reconnect"></a>
### The Reconnect Callback

The _reconnect_ callback is a fine-grained recovery method for
bringing reconnecting clients back into the game. It allows a game
developer to send them any information they might have missed while
disconnected, and also to execute a custom function before their game
is resumed.

The _reconnect_ callback must be specified as a
[step property](https://github.com/nodeGame/nodegame/wiki/Client-Types-v4#step-properties),
and it is executed in the logic whenever a client reconnects. The
function receives two parameters:

* _player_: the object representation of the player that just
  reconnected
* _reconOptions_: a pre-initialized object containing the reconnection
  options to be sent to the reconnecting client.
  
The _reconnect_ callback must decorate the _reconOptions_ object with
any property that is necessary to resume the game properly. The
following properties have a special meaning:

* **plot**: an object whose properties will replace the step
    properties of current step. It is preset with the "timer"
    property, containing the time left in current step.

* **cb**: this function will be called in the reconnecting client just
    before resuming. It receives the whole _reconOptions_ object
    itself as input parameter. Its purpose is to do all local
    modifications to prepare the client for resuming the game.

* **targetStep**: this option is preset by the server, and should not
    be modified. The step of reconnection for the client. Default: the
    step in which the client disconnected, or the step at which the
    logic currently is, if more advanced.
    
* **willBeDone**: this option is preset by the server, and should not
    be modified. If TRUE, the client calls `node.done()` immediately
    after resuming. However, the `done` handler is not invoked, and no
    "set" message is sent to the server (supposedly, the client
    disconnected while being DONE in current step and this information
    has already been sent to the server).

* **beDone**: this option is preset by the server, and should not be
    modified. If TRUE, it makes game done immediately when entering a step (no
    frame loaded, no callback executed like in `willBeDone`).
    
Finally, if the _reconnect_ callback returns FALSE, then the
reconnection procedure is ended immediately and the client disposed.


## What Happens When a Client Attempts to Reconnect

When a client reconnects after a disconnection the following steps are
taken:

1. The game room in which the player was at the moment of
disconnection is looked-up. If no longer existing, the client is
either disposed or treated as a new connection, depending on the
[channel settings](#channel).

2. The stage in which the player disconnected and the current logic's
stage are compared. If `sameStepReconnectionOnly` is enabled in the
channel settings (default FALSE), then the client is disposed if the
stages are different.

3. The client is setup again (the `node.game` object is
recreated). Language settings are also sent, if different from
English.

4. The client is initialized, but the game is not started yet.

5. If a _[reconnect](#reconnect)_ step property is set in the logic,
it is executed. If it returns FALSE, the client is disposed.

6. The reconnection options, as prepared by the _reconnect_ callback
are sent to the client, including the current value of the step timer,
any modification to the plot object.

7. The client is advanced to the proper reconnecting step: either the
step of in which the disconnection had happened, or the current
logic's step, whichever is more ahead in the game sequence.

8. If any size-handler is defined, it is evaluated. If the number of
players is still not correct, the reconnecting client is immediately
paused.

## Replacing Disconnected Players with Bots.

To replace a disconnected player with a bot refer to the [Server
API](Server-API-v4).

## Next

* [Push Manager](PushClients-v4)

## Go Back to 

* [Home](Home)
