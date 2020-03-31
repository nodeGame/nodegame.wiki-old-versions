- status: complete
- version: 3.x
- follows from: [Game Basics](Game-Basics-v3)

All game settings and treatments are contained in file
`game/game.settings`. Examples of the variables contained in this file
are: the number of coins that two players would divide in an ultimatum
game, or the exchange rate between experimental coins and some real
world currency.

Game variables can be grouped together under a common label to create
a _treatment_. Treatments are defined inside a special game variable:
the `treatments` object. All the variables defined outside the
`treatments` object are shared by all treatments; variables defined
inside the `treatments` object are available only to a specific
treatment, and can also overwrite shared variables. In the example
below, "rich_treatment" overwrites the value of variable "COINS":

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

  WAIT_TIME_TEXT: 'Somebody disconnected!',

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
some names are reserved for special purposes. For example,

 * TIMER: an object containing the values (in milliseconds) of the
   timers of each step. If a step with the same of one of the
   properties of the TIMER object is found, then a new timer is
   instantiated with the duration (or options) containing in the TIMER
   object.     
 * WAIT\_TIME: the waiting time (in seconds) for a disconnected player
   to reconnect (see 
 * WAIT\_TIME\_TEXT: the text display to other players while waiting
   for a player to reconnect.

### Assigning a treatment to a game room

When a new game starts, the [waiting room](Waiting-Room-v3) selects
and assigns a treatment among those available. All the game variables
of the selected treatment are automatically sent to every connected
client and stored as `node.game.settings`. Moreover, all treatment's
settings are also available during the definition of client types (see
Section [Client Types](Client-Types-v3)).

#### game.setup.js

The `game.setup` file contains settings that are not directly related
to the game, but rather to the nodeGame engine. For example, if the
game should be executed in _debug_ mode or not, and the frequency of
update messages to exchange with the server.

    
## Next Topics

* Next: [How to create the game sequence](Game-Sequence-v3)
