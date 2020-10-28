- status: complete
- version: 5.x
- follows from: [Game Basics](https://github.com/nodeGame/nodegame/wiki/Game-Basics-v5#directory_structure)

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
were covered in the [Game Basics](Game-Basics-v5) page.

### channel/ folder

The files in this folder configure the technical aspects of the game
channel. NodeGame supports multiple games at the same time. Each game is
contained in a separate folder, and it is assigned an own _channel_. A
channel has the same name of the game, so if your game is called
_myexperiment_, the following address will be reserved to join the
experiment:

    http://localhost:8080/myexperiment/        

For further details and configuration options read the
[channel configuration](Channel-Configuration-v5) page.

### auth/ folder

This folder contains the authorization rules for the channel. The
authorization is cookie-based and details and configuration are
explained [here](Authorization-Rules-v5).


### requirements/ folder

The files in this folder specify the technical requirements for
accessing the channel. Some basic features of the browser are checked,
as well as the connection speed. It is possible to specify custom
requirements as well. Details and configuration are explained
[here](Requirements-Checkings-v5).


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

For a full guide about how to create views see the [Views](Views-v5)
page.

### levels/ folder

Game levels divide an the experiment in independent parts,
each of which can have a separate waiting room and separate
synchronization rules. For example, an experiment could start with a
first part where participants do not interact with each other, but
they only answer some survey questions. Only in part 2, they would
reach a waiting room and form groups for synchronous play. Using game levels may reduce the dropout rate in later parts where synchronous play is happening.  

Read more about [levels](Levels-v5).

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

<a name="package_json_card"></a>
#### Card

The property `card` inside package.json controls how the game is visualized in the home page of nodeGame. All card settings are optional:

- `name`: another name instead of the game name,
- `description`: anothe text instead of the package description,
- `icon`: the name of the icon as found in the [Materialize website](http://materializecss.com/icons.html)
- `wiki`: a url to a wikipedia entry
- `publication`: a url to publication

## Next

* [Channel Configuration](Channel-Configuration-v5)

## Go Back to

* [Home](Home)
- [Game Basics](https://github.com/nodeGame/nodegame/wiki/Game-Basics-v5#directory_structure)
