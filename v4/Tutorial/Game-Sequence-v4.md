- status: complete
- version: 4.x
- follows from: [Settings and Treatments](Settings-and-Treatments-v4)


The _Game Sequence_ is the sequence of the stages and steps of a game:

- step: the basic unit of execution of a game, at least including an
  HTML page, and some code to handle user interaction with the page
  and with the server;
- stage: a collection of steps.

An example of game sequence could be the following. First, the
_instructions_ are shown to the participants; second, a _verification
quiz_ is performed; third, a _game_ is repeated for 3 rounds where the
participants take a _decision_ simultaneously, and then the _results_
are displayed.

The game sequence is defined in file `game/game.stages.js` using the
[Stager-API](Stager-API-v4). Here is how the game sequence described
above would be implemented:

```javascript
// First stage (contains one default step).
stager.stage('instructions');

// Second stage (contains one default step).
stager.stage('verification_quiz');

// Third stage repeats its steps 3 times in the order they are added.
stager.repeatStage('game', 3);
stager.step('decision');
stager.step('results');
```


At this point, The game sequence is still empty, and it must be
implemented in a [client type](Client-Types-v4).

    
## Next Topics

* Next: [Learn how to implement client types](Client-Types-v4)
