- status: complete
- version: 4.x


## Preparation

- Install version 6.x (or higher) of [node.js](http://nodejs.org) for
  your operating system (Win/Mac/Linux).
- Install [git](https://git-scm.com/) for your operating system (Win/Mac/Linux).
- Check that _node_ and _git_ are installed. Open a <a
  href="https://en.wikipedia.org/wiki/Terminal_(macOS)"
  target="_blank">terminal</a> or a <a
  href="https://en.wikipedia.org/wiki/Cmd.exe" target="_blank">command
  prompt</a> and type:
  
  ```
  node --version
  ```
  
  You should see something like: v6.11.2

  ```
  npm --version
  ```
  
  You should see something like: 3.10.10

  ```
  git --version
  ```  

  You should see something like: git version 2.16.2


- **Important**: some Linux distributions install an executable named
  _nodejs_, and it might cause the installation to break. If this is the
  case, make a symbolic link from _nodejs_ to _node_.

## Installation

- Download the [nodeGame installer](https://raw.githubusercontent.com/nodeGame/nodegame/master/bin/nodegame-installer.js) (right-click, save as)
- Open a terminal To install nodeGame v4 type on a terminal: `node nodegame-installer.js @v4`
- Follow instructions on screen


## Running Your First Game

NodeGame has an <a href="https://en.wikipedia.org/wiki/Ultimatum_game"
target="_blank">Ultimatum Game</a> included in the default
installation. To start the server and play the game follow these
steps:

1. Open a terminal and navigate to the `nodegame` folder
2. Start the server with the command: `node launcher.js`
3. Open one browser tab pointing to: `localhost:8080`
4. Open more tabs, click "Play with Bots", or manually launch a bot connecting
   to: `localhost:8080/ultimatum/?clientType=autoplay`
5. Check the monitor interface: `localhost:8080/ultimatum/monitor`


## Next Topics

Next:
[Learn about how to create new games in nodeGame](Game-basics-v4)
