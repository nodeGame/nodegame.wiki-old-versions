- status: complete
- version: 5.x
- follows from: [Create New Game](Create-New-Game-v5)

## Directory Structure: Basic

The game folder contains the following sub-folders and files:

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

In this basic guide we focus only on the items in bold, they are
enough to get you started. For information about the other folders and
files refers to the [Game Advanced](Game-Advanced-v5) guide.

### game/ folder

The files in the `game` folder define the behavior of the game: the
default settings and available treatments, the sequence of game stages
and their implementation in different client types. The following
files and sub-folders are available:

* game.settings.js
* game.stages.js
* game.setup.js
* client_types/

Below we give a brief overview, but we will come back with more details at later.

#### game.settings.js

The `game.settings` file contains all the game variables. Examples of
game variables are: the timer for each step, the number of coins to win, or the exchange rate between experimental coins and some real world currency. Variables can be grouped together to create treatments. More details in the
[settings and treatments page](Settings-and-Treatments-v5).

#### game.stages.js

The `game.stages` file defines the _sequence_ of stages and steps
of the game. More details in the
[game sequence page](Game-Sequence-v5).

#### game.setup.js

The `game.setup` file is optional and it can be used to load
additional resources that are shared across all game rooms. For
example, a database of images' path should be imported here.

Technical note! The return value of this file is _not_ sent to all connected clients, but it is available only to the logic client types (see next section). Moreover, it is "copied by reference," so that any modification will be shared across all game rooms too.

<a name="client_types"></a>
#### The client_types/ folder

This folder contains the implementation of the sequence for different
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

For more details, refer to the [client types page](Client-Types-v5).

### The public/ folder

The `public/` folder contains all the static files that will be served
by the game; it must contain a file named `index.htm` that will be served first to connecting clients.

Please note:

- All files inside this directory are accessible through the browser. _Do not store sensitive information in the `public/` folder_.

- When running a game as _default_, sub-directories in `public/` may overshadow nodeGame's default directories. Default directories are:

    - `fonts/`,  `images/`,  `javascripts/`,  `lib/`,  `pages/`,  `sounds/`, `stylesheets/`.

For instance, adding a `lib/` folder inside `public/` will overshadow the nodeGame's `lib/` folder, from which libraries such as Bootstrap are served.

#### Views

If you wish to create HTML pages dynamically, for example go customize
the language of each page, you may use [views](Views-v5), which are explained in the advanced guide.

### The waitroom/ folder

The `waitroom/` folder contains the waitroom and the waitroom
settings.

The `waitingroom.settings.js` file controls the behavior of the
waiting room. The full list of options is described in the
[Waiting Room](Waiting-Room-v5) page.


## Next

* [Define game variables and treatments](Settings-and-Treatments-v5)
