- status: complete
- version: 3.x
- follows from: [Getting Started](Getting-Started-v3)

## Create a New Game Directory

Inside your nodegame installation folder type

```
   cd bin
   node nodegame create-game gamename
```

This will create a new game folder named _gamename_ containing all the
necessary files that you can then modify to suit your needs.

Important! If your installation does not contain `bin/nodegame` file
(usually on Windows), you can manually download the <a
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
<a name="directory_structure"></a>
## Directory Structure

When you create a new game with the nodegame-generator utility the
game folder will contain the following sub-folders and files:

Basics:

* **game/**
* **public/**
* **waitroom/**

Advanced:

* auth/
* channel/
* data/
* requirements/
* views/
* levels/
* package.json

In this basic guide we focus only on the items in bold. They are
enough to get you started. For information about the other folders and
files refers to the [Game Advanced](Game-Advanced-v3) page.

### game/ folder

The files in the `game` folder define the behavior of the game: the
default settings and available treatments, the sequence of game stages
and their implementation in different client types. The following
files are available:

* game.settings.js
* game.stages.js
* game.setup.js
* client_types/

#### game.settings.js

The `game.settings` file contains all the game variables. Examples of
game variables are: the number of coins that two players would divide
in an ultimatum game, or the exchange rate between experimental coins
and some real world currency. More details in the
[settings and treatments page](Settings-and-Treatments-v3).

#### game.stages.js

The `game.stages` file defines the _sequence_ of stages and steps
of the game. More details in the
[game sequence page](Game-Sequence-v3).

#### game.setup.js

The `game.setup` file is optional and it can be used to load
additional settings and to share them across all game rooms. For
example, a large array of file paths loaded from file system should be
imported here. Important! The return value of this file is _not_ sent
automatically to all connected clients, but it is rather made
available only to the logic client types (see next section). Moreover,
it is copied by reference, so that any modification will be shared
across all game rooms too.

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

For more details, refer to the [client types page](Client-Types-v3).

### The public/ folder

The `public/` folder contains all the static files that will be served
by the game. A file `index.htm` must be found here.

If you wish to create HTML pages dynamically, for example customizing
the language of each page, you should use the `views/` folder. Views
are explained in the [views page](Views-v3) of the advanced guide.

### The waitroom/ folder

The `waitroom/` folder contains the waitroom and the waitroom
settings.

The `waitingroom.settings.js` file controls the behavior of the
waiting room. The full list of options is described in the
[Waiting Room](Waiting-Room-v3) page.

    
## Next

* [Learn how to define game variables and treatments](Settings-and-Treatments-v3)
