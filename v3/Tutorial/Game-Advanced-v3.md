- status: complete
- version: 3.x
- follows from: [Game Basics](https://github.com/nodeGame/nodegame/wiki/Game-Basics-v3#directory_structure)

## Directory Structure

When you create a new game with the nodegame-generator utility the
game folder will contain the following sub-folders and files:

Basic:

* game/
* public/
* waitroom/

Advanced:

* **channel/**
* **auth/**
* **requirements/**
* **views/**
* **levels/**
* **data/**
* **package.json**

In this advanced guide we focus on the items in bold. The other items
were covered in the [Game Basics](Game-Basics-v3) page.

### channel/ folder

The files in this folder configure the technical aspects of the game
channel. NodeGame supports multiple games at the same time. Each game is
contained in a separate folder, and it is assigned an own _channel_. A
channel has the same name of the game, so if your game is called
_myexperiment_, the following address will be reserved to join the
experiment:

    http://localhost:8080/myexperiment/        
    
For further details and configuration options read the
[channel configuration](Channel-Configuration-v3) page.
    
### auth/ folder

This folder contains the authorization rules for the channel. The
authorization is cookie-based and details and configuration are
explained [here](Authorization-Rules-v3).


### requirements/ folder

The files in this folder specify the technical requirements for
accessing the channel. Some basic features of the browser are checked,
as well as the connection speed. It is possible to specify custom
requirements as well. Details and configuration are explained
[here](Requirements-Checkings-v3).


### data/ folder

In this directory is where the results of the experiments are supposed
to be saved. The experimenter must specify what to save.

### views/ folder

Views are dynamically created HTML pages. The files in this folder are
divided in two sub-folders:

* templates/
* contexts/

Templates are [jade](http://jade-lang.com/) files which define the
structure of the page. Contexts contain the variables used to fill in
the content of the templates.

For a full guide about how to create views see the [Views](Views-v3)
page.

### levels/ folder

Game levels allow the division of the experiment in multiple parts,
each of which can have a separate waiting room and separate
synchronization rules. For example, an experiment could start with a
first part where participants do not interact with each other, but
they only answer some survey questions. Only in part 2, they would
reach a waiting room and form groups for synchronous play. Introducing
multiple game levels has the advantage to reduce the number of
dropouts in later parts, for example where synchronous play is
happening.  

To create a new game level, create a new folder with the name of the
level -- e.g. "part2" -- inside the `levels/` directory. Then, inside
the new game level directory, add a `game/` folder, and optionally a
`waitroom/` folder, following exactly the same structure as for the
folders with the same name at the top-level of the directory.

Moving clients between levels must be done manually. For example, the
client finishing a level could send a message to the logic. Upon
receiving it, the logic would then use the [Server API](Server-API-v3)
to move the client to next room (usually the waiting room of the next
level).

```javascript
node.on.data('level_done', function(msg) {
    var levelName = 'part2';
    var roomName = 'part2WaitRoom';
    // Move client to the next level.
    // (async so that it finishes all current step operations).
    setTimeout(function() {
        console.log('moving client to next level: ', msg.from);
        channel.moveClientToGameLevel(msg.from, levelName, roomName);
    }, 100);
});
```

### package.json

This file contains general information such as the name of the game,
its version and the name of author. The same file is used by [npm](
https://www.npmjs.com/) for packaging the game as a publishable module.


```javascript
{
    "name": "ultimatum",
    "version": "0.1.0",
    "description": "This is an ultimaum game",
    "author": "Author Name <author@email.com>",
    "license": "MIT/X11",
    "homepage": "http://nodegame.org"
}
```

## Next

* [Channel Configuration](Channel-Configuration-v3)

## Go Back to 

* [Home](Home)
- [Game Basics](https://github.com/nodeGame/nodegame/wiki/Game-Basics-v3#directory_structure)
