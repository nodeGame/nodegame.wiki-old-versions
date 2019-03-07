- status: complete
- version: 2.x
- follows from: [Getting Started](Getting-Started-v2)

## Create a New Game Directory

Inside your nodegame installation folder type

```
   cd bin
   node nodegame create-game gamename
```

This will create a new game folder named _gamename_ containing all the
necessary files that you can then modify to suit your needs.

Important! If your installation does not contain `bin/nodegame` file,
you can manually download <a
href="https://github.com/nodeGame/nodegame-generator"
target="_blank">nodegame-generator</a> package. In a directory of your
choice type the following commands:

```
    git clone https://github.com/nodeGame/nodegame-generator.git
    cd nodegame-generator
    npm install
    cd bin
    node nodegame create-game gamename
```
<a name=directory_structure"></a>
## Directory Structure

When you create a new game with the nodegame-generator utility the
game folder will contain the following sub-folders and files:

* **game/**
* **public/**
* **waitroom/**
* auth/
* channel/
* data/
* requirements/
* views/
* levels/
* package.json

In this basic guide we focus only on the items in bold. They are
enough to get you started. For information about the other folders and
files refers to the [Game Advanced](Game-Advanced-v2) page.

### game/ folder

The files in the `game` folder define the behavior of the game: the
default settings and available treatments, the sequence of game stages
and their implementation in different client types. The following
files are available:

* game.settings.js
* game.setup.js
* game.stages.js
* client_types/

#### game.settings.js

The `game.settings` file contains all the game variables. Examples of
game variables are: the number of coins that two players would divide
in an ultimatum game, or the exchange rate between experimental coins
and some real world currency.

It is possible to group together game variables under a common label
to define a _treatment_. Treatments are defined inside a special game
variable: the `treatments` object. Each treatment inherits all the
default game variables (defined outside the `treatments` object). In
addition, treatments can also define own game variables, or overwrite
default ones. For example:

```javascript

{
  // Default game variables are specified here.

  COINS: 100,

  EXCHANGE_RATE: 0.0001,

  TIMER: {
      instructions: 60000,
      bidder: 30000,
  },

  WAIT_TIME: 30,

  // The treatments object is a special default variable
  // which contains treatments objects.
  treatments: {

      // Name of the treatment.
      'rich_treatment': {

          // Overwrites the default variable COINS,
          // but still inherits EXCHANGE_RATE.
          COINS: 1000,

          // Defines a new variable named TEXT.
          TEXT: "You are rich now!"

      }

      // Other treatments would follow below.

  }
}
```

Game developers can define as many game variables as needed, however
some names are reserved for special purposes. They are:

 * TIMER: an object containing the values (in milliseconds) of the
   timers of each step. If a step with the same of one of the
   properties of the TIMER object is found, then a new timer is
   instantiated with the duration (or options) containing in the TIMER
   object.
 * WAIT_TIME: the default waiting time (in seconds) for a disconnected
   player to reconnect.

##### Treatments

When a new game starts, the waiting room assigns a treatment to
it. All the game variables are sent automatically to every connected
client and are available through the object `node.game.settings`.
Moreover, treatment's settings are also available during the
definition of client types (see Section
[Client Types](Client-Types-v2)).

#### game.setup.js

The `game.setup` file contains settings that are not directly related
to the game, but rather to the nodeGame engine. For example, if the
game should be executed in _debug_ mode or not, and the frequency of
update messages to exchange with the server.

#### game.stages.js

The `game.stages.js` file defines the _sequence_ of stages and steps
of the game.

An example of game sequence would be: first stage _instructions_,
second stage a _verification quiz_, third stage _game_ with two nested
steps _decision_ and _results_ repeated 3 times each. Here is how it
would be implemented in code:

```javascript
stager.stage('instructions');
stager.stage('verification_quiz');
stager.repeatStage('game', 3);
stager.step('decision');
stager.step('results');
```

For a complete guide about how to define game sequences refer to the
[Stager API](Stager-API-v2).

<a name="client_types"></a>
#### The client_types/ folder

The folder contains the implementation of the sequence in different
client types. Each client type goes through the same sequence, but can
execute completely different code.

You can have as many client types as you wish for, but at least two
are mandatory: the `player` client type and the `logic` client type.

The `player` client type is what is going to be sent to the players
connecting with their browser to the game server.

The `logic` client type is executed on the server and handles
operation like matching of players, computing the bonus, etc.

Each client type must be saved in its own file that determines also
the name of the client type, that is the `player` client type is saved
in the `player.js` file.

For a complete guide about how to implement client types refer to the
page on [Client Types](Client-Types-v2).

### The public/ folder

The `public/` folder contains all the static files that will be served
by the game. At least one index file must be specified here.

If you wish to create HTML pages dynamically, for example customizing
the language of each page, you should use the `views/` folder. See the
[Views](Views-v2) page for help.

### The waitroom/ folder

The `waitroom/` folder contains the waitroom and the waitroom
settings.

The `waitingroom.settings.js` file controls the behavior of the
waiting room. The meaning of the main options is described below.

* EXECUTION\_MODE: (string) The execution mode of the waiting
room. Different modes might have different default values, and require
different settings. Available modes:
   - ´TIMEOUT´, waits until the time is up, then it checks
        whether enough players are connected to start the game.
   - ´WAIT_FOR_N_PLAYERS´, the game starts right away as soon as
        the desired number of connected players is reached.

* POOL\_SIZE: (integer) Waits for at least POOL_SIZE players before
  starting a new game.

* GROUP\_SIZE: (integer) When enough players are connected, GROUP_SIZE
  players will be moved inside the new game room. GROUP_SIZE must be
  smaller or equal to POOL_SIZE, and multiple game rooms will be
  created at the same time if POOL_SIZE is a multiple of
  GROUP\_SIZE. If POOL_SIZE is larger than GROUP_SIZE, but not a
  multiple, the players that will be moved inside the game room will
  be selected randomly, and the remaining will continue waiting until
  conditions for creating new game rooms are met again.

* CHOSEN\_TREATMENT: (string|undefined) The name of the treatment that
  will be chosen for the game rooms. If undefined, a random treatment
  will be chosen. If equal to `treatment_rotate`, treatments will be
  assigned sequentially to new game rooms.

* MAX\_WAI\T_TIME: (integer|undefined) The maximum number of
  milliseconds a player is allowed to wait in the waiting room. If
  set, the player will be disconnected if the timer expires.

* ON\_TIMEOUT: (function) If set, this function will be executed on the
  clients when the MAX\_WAIT\_TIME timer expires.

* ON\_TIMEOUT\_SERVER (function) If set, this function will be
  executed on the server the the MAX\_WAIT\_TIME timer expires. The
  context of execution is `WaitingRoom`.
    
* START\_DATE: (string|object) Overrides `MAX_WAIT_TIME`. Accepted
values are any valid argument to `Date` constructor, for example:
`December 24, 2016 23:59:59`, or `new Date().getTime() + 30000`,

* DISPATCH\_TO\_SAME\_ROOM: (boolean) If TRUE, every new group will be
  added to the same game room -- the one created first. Default,
  FALSE. Notice the game must support adding players while it is
  running.
  
* logicPath: (string) If set, a custom implementation of the wait room
  will be used.
    
## Next Topics

* Next:
  [Learn how to implement client types for your game](Client-Types-v2)
