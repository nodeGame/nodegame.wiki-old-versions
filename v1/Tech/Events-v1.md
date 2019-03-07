# Events

- status: complete
- version: 1.x

## Introduction

NodeGame is an asynchronous
[event-driven](http://en.wikipedia.org/wiki/Event-driven_programming)
framework, meaning that the flow of execution is determined by events
such as user actions (mouse clicks, key presses), and / or incoming
messages from the server.

Events are emitted using `node.emit()` and caught using `node.on()`.

It is possible to define any custom event, but some already reserved
by the nodeGame engine. The complete list follows.

## Alphabetical List

Important! Some events can be disabled / enabled by the server and
client configuration options.

### Incoming Messages

The following events are fired whenever an incoming message is
received, and contain the game message as input parameter `msg`.

- **in.say.ALERT**: an alert message is going to be displayed. Alert
    text in `msg.text`.

- **in.say.DATA**: generic data message. It is possible to use the
    alias `node.on.data` to catch any data message based on the label
    of the data found in `msg.text`.

- **in.say.HI**: first message sent from the server to the client,
    upon successful connection. The message contains the client id and
    a welcome message in `msg.data`. Before this message is received,
    other messages will be discarded.

- **in.say.GAMECOMMAND**: a game command was received. If the command
    is valid, it will be emitted as `NODEGAME_GAMECOMMAND` and

- **in.set.LANG**: a game command setting the language of the client
    was received. `LANGUAGE_SET` will be emitted. 

- **in.say.MCONNECT**: an admin (monitor) connected to the same game
    room. The client data is contained in `msg.data`. This message is
    sent only to other administrators. Will emit `UPDATE_MLIST`.

- **in.say.MDISCONNECT**: an admin (monitor) disconnected from the
    same game room. The client data is contained in `msg.data`. This
    message is sent only to other administrators. Will emit
    `UPDATE_MLIST`.

- **in.say.MLIST**: a game command containing a new list of currently
    connected admins (monitors) is received. The list is contained in
    `msg.data`. This message is sent only to other administrators.

- **in.say.MRECONNECT**: a previously disconnected admin (monitor)
    reconnected. There is no default handler at the moment, and if one
    is defined nothing will happen, and the monitor will not be
    automatically re-added to the monitor list.

- **in.say.PCONNECT**: a client (player) connected to the same game
    room. The client data is contained in `msg.data`. Will emit
    `UPDATE_PLIST`.

- **in.say.PDISCONNECT**: a client (player) disconnected from the same
    game room. The client data is contained in `msg.data`. Will emit
    `UPDATE_PLIST`.

- **in.say.PLAYER_UPDATE**: a player update is received from a client
    connected to the same game room. The update data is contained in
    `msg.data`, and the client-id of the client to update is contained
    in `msg.from`. Will emit `UPDATE_PLIST`.

- **in.say.PLIST**: a game command containing a new list of currently
    connected clients (players) is received. The list is contained in
    `msg.data`. Will emit `UPDATE_PLIST`.

- **in.say.PRECONNECT**: a previously disconnected client (player)
    reconnected. There is no default handler at the moment, and if one
    is defined nothing will happen, and the client will not be
    automatically re-added to the player list.

- **in.say.REDIRECT**: redirects a client connected remotely to a new
    uri. The uri of the new location is contained in `msg.data`. If
    the window-object is not found an error message will be logged.

- **in.say.SERVERCOMMAND**: this message has no effect on the client,
    but it is used to control the server, and to receive information
    back. It is available to administrators only, with the following
    options:

    - **ROOMCOMMAND**: execute a room command to all or a subset of
        clients. The room command (SETUP, START, PAUSE, RESUME, STOP)
        must be specified in `msg.data.type`, the room-id in
        `msg.data.roomId`. Optionally, it is possible to specify the
        id of the clients in which the commands should be executed in
        an array in `msg.data.clients`, if the logic of the game room
        should be included or not in `msg.data.doLogic`, and whether
        the operation should be forced, if not allowed, in
        `msg.data.force`.      
    
    - **STARTBOT**: starts a bot on the server. At the moment only
        PhantomJS bots can be started. Configuration options must be
        passed in `msg.data`. See
        [Server Configuration](Server-Configuration) for details.
    
    - **INFO**: request information from the server. The type of
        information requested is specified in `msg.data.type` and it
        can assume any of the values: CHANNELS, ROOMS, CLIENTS, GAMES.
    
- **in.say.SETUP**: executes the registered setup function for the
    requested feature with the given data payload. The feature name is
    contained in `msg.text`, and the data payload is in `msg.data`. If
    no payload is found an error message will be logged. See the
    [Client Configuration](Client-Configuration) for which features
    are available for setup.

### Internal Events

The following events are emitted when something happens internally (no
trigger from the server). Examples are: a callback function has been
executed, a new stage has been loaded, the game has been paused, etc.

- **BEFORE_PLAYING**: the client is about to emit `PLAYING`. The
    control has not yet been yielded to the player / bot and the
    buffer has not yet been cleared. See `PLAYING`.

- **BEFORE_GAMECOMMAND**: a game command was received, and it will
    executed next. The name of the game command is passed as input
    parameter to the handler, possibly with additional options. Game
    commands described in `NODEGAME_GAMECOMMAND_XXX`.
    
- **DONE**: the current step / stage is done, and the game _should_
    continue to the next one. If a `done` handler is registered in the
    stager, the function will be executed. If the `done` handler
    returns TRUE, the `STAGE_LEVEL` will be set to 100 (DONE), and the
    `REALLY_DONE` event will be emitted. See `REALLY_DONE`.

- **INIT**: the initialization function is about to be executed. See
    `game.start()`.

- **GAME_ALMOST_OVER**: the `GAME_OVER` event is about to be
    fired. This event handler is fired _before_ the game-over handler
    is executed. See `node.game.gameover`.

- **GAME_OVER**: the game has reached the end of the sequence. The
    game-over handler has been exeucted. It will not be possible to
    step forward. See `node.game.gameover`.

- **LANGUAGE_SET**: the language of the client has been set. See
    `node.setLanguage`.

- **LOADED**: the game-window has finished loaded a frame. If this
    happens _after_ the `STEP_CALLBACK_EXECUTED` event has been fired
    already, the `PLAYING` event will be fired subsequentely. See
    `node.window.loadFrame`.

- **NODEGAME_GAMECOMMAND_XXX**: where XXX is one of the options below:
    
    - **clear_buffer**: the client received the game
        command to clear the buffer of previously received messages. All
        buffered messages will be emitted. `node.socket.clearBuffer`.
    
    - **erase_buffer**: the client received the game
        command to erase the buffer of previously received messages,
        without emitting them. See `node.game.eraseBuffer`.
        
    - **goto_step**: the client received the game
        command to go to a specific step. If the game is not steppable, or
        the step is not-existing in the client, an error message will be
        logged. See `node.game.gotoStep`.
    
    - **pause**: the client received the game command
        to pause the game. If the game cannot be paused, an error message
        will be logged. See `node.game.pause`.
    
    - **restart**: the client received the game
        command to restart the game. If the game cannot be restarted, an
        error message will be logged. See `node.game.restart`.
        
    - **resume**: the client received the game
        command to resume the game. If the game cannot be resumed, an
        error message will be logged. See `node.game.resume`.
    
    - **start**: the client received the game command
        to start the game. If the game cannot be started, an error message
        will be logged. See `node.game.start`.
    
    - **step**: the client received the game command
        to go to the next step. If the game is not steppable, an error
        message will be logged. See `node.game.step`.
    
    - **stop**: the client received the game command
        to stop the game. If the game cannot be started, an error message
        will be logged. See `node.game.stop`.    

- **NODEGAME_READY**: the client has received a `HI` message from the
    server, and it is ready to create the session. This handler is
    executed _before_ the session and the player are actually created
    (unless diversely initialized).

- **PAUSING**: the client is about to be paused. This handler is
    executed _before_ a pause event-handler is set, and before
    `PAUSED` is emitted. See `PAUSED` and `node.game.pause`.

- **PAUSED**: the client is paused. A pause event-handler has been
    set, and only messages with high priority will be emitted directly
    and the others will be buffered. See `node.socket.onMessageFull`.

- **PLAYER_CREATED**: the player was created in `node.player`. From
    now on, messages can be sent to server. See `node.createPlayer`
    and `node.startSession`.

- **PLAYING**: the client is releasing control to the user / bot. This
    event is executed for every new stage / step, after the step
    callback is executed and the window has finished to load all
    frames (if executed in the browser at all). _After_ this event is
    fired, it is possible again to send and received messages
    normally; before this event, messages received will be buffered.
    See `LOADED`, and `STEP_CALLBACK_EXECUTED`.

- **REALLY_DONE**: the stage / step was terminated, and the `done`
    handler returned TRUE. The game will now determine if it _should_
    continue immediately with the next step, or wait for other clients
    to be done as well.

- **RESUMING**: the client is being resumed, after being paused. This
    handler is executed while the game is still paused, and _before_
    restoring the default message handler. See `node.game.resume`, and
    `RESUMING`.

- **RESUMED**: the client was resumed. Normal message-handler is in
    place again. See `node.game.resume`. 

- **STEPPING**: the client is about to step into the next stage. What
    follows next is: stage is updated, old event-handlers are cleared,
    and the new step will be executed. See `node.game.step`.

- **STEP_CALLBACK_EXECUTED**: the step callback has been
    executed. _After_ this event handler, the `LOADED` event might be
    invoked. See `LOADED`.

- **SOCKET_CONNECT**: a socket connection is established. Under normal
    conditions, this happens _after_ the player object has been
    created, therefore it is usually not possible to send game
    messages immediately after catching this event. See `node.socket.connet.`.

- **SOCKET_DISCONNECT**: a previously established connection is
    terminated. It will not be possible to send game or receive game
    messages until a new `SOCKET_CONNECT` is emitted.

- **UPDATED_PLIST**: one client communicated a change in his state,
    and the change was applied to the player list in
    `node.game.pl`. Updates includes: connections, disconnection,
    change of stage-level, change of state-level, or if the player
    list is replaced completely. After an `UPDATED_PLIST` event, it is
    always checked if the game should step, or if the `PLAYING` event
    should be emitted. See `node.game.pl`, and `PLAYING`.


### Outgoing Messages

All outgoing messages will fire an event of the type
_.[say|get|set].[target]_ in debug mode. (Note: this might become a
configuration option `emitOutMsg` in the future.)
