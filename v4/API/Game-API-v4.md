- status: complete
- version: 4.x

The _Game API_ controls the flow of the game. It is a subset of the
[Client API](Client-API-v4) and it is accessible through the object
`node.game`. The most common methods and properties are described
below.

### Game properties

- `game.memory`<br>
   Where the game results are saved. It is a special
   instance of the _NDDB_ database. For more information see the
   [Memory-Database page](Memory-Database-v4) or the
   [NDDB home page](http://nodegame.github.com/NDDB/).
- `game.pl`<br>
   The list of players currently connected. For more information see
   [Player-List page](PlayerList-v4).
- `game.timer`<br>
   The default game timer containing how much time is left in current step.
- `game.settings`<br>
   The object containing the current treatment's settings (in remote clients).
- `node.plot`<br>
   The object containing the game sequence.
- `node.pushManager`<br>
   The object responsible for making the game keep up with its
   timer. For more information see the [push-manager page](Push-Manager-v4)
- `node.sizeManager`<br>
   The object responsible for taking actions when the number of
   connected players changes. For more information see the page on 
   [how to handle disconnections](Handling-Disconnections-v4).

### Game methods

#### Fetching properties of the game sequence

- **Game.getProperty(name)**: Returns the requested step property from
    the game plot
- **Game.getCurrentStepObj()**: Returns the current step object from
    the game plot.
    
#### Get current/previous/next game-stage

- **Game.getCurrentGameStage()**: Return the GameStage that is
    currently being executed.
- **Game.getPreviousStep(delta)**: Returns the game-stage that was
    played delta steps ago
- **Game.getNextStep(delta)**: Returns the game-stage that will be
    played in delta steps
- **Game.compareCurrentStep(gameStage)**: compare the given game stage
    against current game stage.

#### Altering the game flow

- **Game.pause()**: Pauses the game.
- **Game.resume()**: Resumes the game.
- **Game.stop()**: Stops the game.

#### Checking the state of the game

- **Game.isReady()**: Returns TRUE, if game is fully initialized.
- **Game.isPaused()**: Returns TRUE, if game is paused.
- **Game.isGameover()**: Returns TRUE, if game over has been called.
- **Game.isStoppable()**: Returns TRUE, if game can be stopped.
- **Game.isPausable()**: Returns TRUE, if game can be paused.
- **Game.isResumable()**: Returns TRUE, if game can be resumed.
- **Game.isSteppable()**: Returns TRUE, if game can be stopped.


## Sources

* [Game.js](https://github.com/nodeGame/nodegame-client/blob/master/lib/core/Game.js)
* [Inline Documentation](http://nodegame.github.io/nodegame-client/docs/lib/core/Game.js.htm)
