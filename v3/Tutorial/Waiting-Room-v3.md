- status: complete
- version: 3.x
- follows from: [Event Listeners](Event-Listeners-v3)

The waiting room is the component responsible for dispatching new
games when certain criteria are met. It is highly configurable, and
its settings are stored in file `waitroom/waitroom.js`.

### Basic Options

* **EXECUTION\_MODE**: (string) The execution mode of the waiting
room. Each mode requires different settings, and might interpret the
same option in a different way. Available modes:

   - `WAIT_FOR_N_PLAYERS`: a new game starts as soon as the desired
        number of players is connected.
   - `TIMEOUT`: a date and a time in the future is scheduled (the
        waiting room still checks whether enough players are connected
        to start the game).

* **POOL\_SIZE**: (integer|undefined) number of players that must be
  connected to start a new dispatch. If undefined, it is equal
  to `GROUP_SIZE`.

* **GROUP\_SIZE**: (integer) When `POOL_SIZE` players are connected,
  then a group of `GROUP_SIZE` players is formed and moved into a new
  game room. `GROUP_SIZE` must be smaller or equal to `POOL_SIZE`. If
  `POOL_SIZE` is larger than `GROUP_SIZE`, then multiple game rooms
  are be created at the same time. If `POOL_SIZE` is not an exact
  multiple of `GROUP_SIZE`, then the players to move inside the new
  game room/s are randomly selected; those left out continue waiting
  until conditions for creating new game room are met again.

* **CHOSEN\_TREATMENT**: (string|function|undefined) The name of the
  treatment that will be chosen for the game rooms. If undefined, a
  random treatment is chosen. If equal to `treatment_rotate`,
  treatments are sequentially assigned to new game rooms. If function,
  it must return the name of the chosen treatment. For example:

  ```javascript
  function(treatments, roomCounter) {
      return treatments[roomCounter % treatments.length];
  }
  ```

* **MAX\_WAIT\_TIME**: (integer|undefined) The maximum number of
  milliseconds a player is allowed to wait in the waiting room. If
  set, the player will be disconnected if the timer expires.

* **ON\_TIMEOUT**: (function) If set, this function will be executed on the
  clients when the MAX\_WAIT\_TIME timer expires.

* **ON\_TIMEOUT\_SERVER**: (function) If set, this function will be
  executed on the server the the MAX\_WAIT\_TIME timer expires. The
  context of execution is `WaitingRoom`, and takes as parameter a
  player object.

* **START\_DATE**: (string|object) Overrides `MAX_WAIT_TIME`. Accepted
values are any valid argument to `Date` constructor, for example:
`December 24, 2016 23:59:59`, or `new Date().getTime() + 30000`,

* **DISPATCH\_TO\_SAME\_ROOM**: (boolean) If TRUE, every new group will be
  added to the same game room -- the one created first. Default,
  FALSE. Notice the game must support adding players while it is
  running.

* **DISCONNECT\_IF\_NOT\_SELECTED**: (boolean) If TRUE, it disconnects
  all clients not selected for a dispatch. (Default: FALSE)

### Events Options

All callbacks receive as first parameter the waiting room object itself.

* **ON\_OPEN**: (function) Callback executed when the waiting room
    becomes "open". Receives as first parameter the waiting room object itself.

* **ON\_CLOSE**: (function) Callback executed when the waiting room
    becomes "close".

* **ON\_CONNECT**: (function) Callback executed when a client connects. Receives
    a player object as second parameter.

* **ON\_DISCONNECT**: (function) Callback executed when a client disconnects.
    Receives a player object as second parameter.

* **ON\_INIT**: (function) Callback executed after settings have been parsed.

* **ON\_DISPATCH**: (function) Callback executed just _before_ starting a
    new dispatch. Receives the options of the dispatch call as second parameter.

* **ON\_DISPATCHED**: (function) Callback executed at the end of a dispatch
    call. Receives the options of the dispatch call as second parameter.

* **ON\_FAILED\_DISPATCH**: (function) Callback executed if a dispatch
    attempt fails. Receives the options of the dispatch call as
    second parameter, and an error message as third parameter (when available).

### Ping Options

Before every dispatch, all connected clients are pinged to make sure
their still responsive.

* **PING_BEFORE\_DISPATCH**: (boolean) If TRUE, all players are pinged
     before a dispatch and non-responding clients are
     disconnected. This option is particularly useful if 
     `POOL_SIZE` is large. Notice: pinging is skipped if 
     mode is `WAIT_FOR_N_PLAYERS` and `POOL_SIZE` is 1. (Default: TRUE)

* **PING\_MAX\_REPLY\_TIME**: (number > 0) The number of milliseconds
    to wait for a reply from a PING. (Default: 3000)

* **PING\_DISPATCH\_ANYWAY** (boolean): If TRUE, dispatch continues even
   if disconnections occur during the PING procedure. (Default: FALSE)

### Grouping and Sorting Options

* **PLAYER\_SORTING**: (function) Sorts the order of players before
   dispatching them. Sorting takes place only if the number of
   connected players > `GROUP_SIZE` and option `PLAYER_GROUPING` is
   undefined. Accepted values:
   - "timesNotSelected": (default) gives priority to players that
        have not been selected in previous dispatch calls
   - undefined: rollback to default choice
   - null: no sorting (players are anyway randomly shuffled).
   - function: a comparator function implementing a criteria
       for sorting two objects. E.g:
    ```javascript
    function timesNotSelected(a, b) {
        if ((a.timesNotSelected || 0) < b.timesNotSelected) {
            return -1;
        }
        else if ((a.timesNotSelected || 0) > b.timesNotSelected) {
            return 1;
        }
        return 0;
    }
    ```

* **PLAYER\_GROUPING**: (function) Creates groups of players to be
     assigned to treatments. This method is alternative to "sorting"
     and will be invoked only if the number of connected players >
     `GROUP_SIZE`. For example:
     ```javascript
     function(playerList, groupsRequired) {
         return [ [ pl1, pl2 ] [ pl3, pl4 ] ];
     }
     ```

### Advanced Options

* **logicPath**: (string) The path to a custom implementation of the
  wait room logic.


## Next

* [Game Advanced](Game-Advanced-v3)

## Sources

 * [Waiting Room Logic](https://github.com/nodeGame/nodegame-game-template/blob/master/waitroom/waitroom.js)
 * [Waiting Room Server](https://github.com/nodeGame/nodegame-server/blob/master/lib/rooms/WaitingRoom.js)

## Go Back to

* [Home](Home)
