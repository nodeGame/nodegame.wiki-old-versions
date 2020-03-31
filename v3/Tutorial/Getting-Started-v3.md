- status: complete
- version: 3.x

## Preparation

- Install version 4.x (or higher) of [node.js](http://nodejs.org) for
  your platform.
- Optionally, install [git](https://git-scm.com/) for your platform
  (necessary for nodeGame's latest version).
- Check that _node_ and _git_ are installed. Open a <a
  href="https://en.wikipedia.org/wiki/Terminal_(macOS)"
  target="_blank">terminal</a> or a <a
  href="https://en.wikipedia.org/wiki/Cmd.exe" target="_blank">command
  prompt</a> and type:
  
  ```
  node --version
  ```
  
  You should see something like: v4.3.2

  ```
  npm --version
  ```
  
  You should see something like: 2.14.12  

  ```
  git --version
  ```  

  You should see something like: git version 1.7.7


- **Important**: some Linux distributions install an executable named
  _nodejs_, and it might cause the installation to break. If this is the
  case, make a symbolic link from _nodejs_ to _node_.

## Installation

### Stable version (latest version below)


- Download the [nodeGame installer](https://raw.githubusercontent.com/nodeGame/nodegame/master/bin/nodegame-installer.js) (right-click, save as)
- To install nodeGame v3 type on a terminal: `node nodegame-installer.js @v3`
- Follow instructions on screen

## Running Your First Game

NodeGame has an <a href="https://en.wikipedia.org/wiki/Ultimatum_game"
target="_blank">Ultimatum Game</a> included in the default
installation. To start the server and play the game follow these
steps:

1. Open a terminal and navigate to the `nodegame` folder
2. Start the server with the command: `node launcher.js`
3. Open one browser tab pointing to: `localhost:8080/ultimatum`
4. Launch a bot connecting to: `localhost:8080/ultimatum/?clientType=autoplay`
5. Check the monitor interface: `localhost:8080/ultimatum/monitor`


## Next Topics

Next:
[Learn about how to create new games in nodeGame](Game-basics-v3)
