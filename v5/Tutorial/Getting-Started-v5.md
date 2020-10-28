- status: complete
- version: 5.x


## Preparation

- Install version 10.x (or higher) of [Node.js](http://nodejs.org) for
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

  You should see something like: v12.15.0

  ```
  npm --version
  ```

  You should see something like: 6.13.4

  ```
  git --version
  ```  

  You should see something like: git version 2.22.0


- **Important**: some Linux distributions install an executable named _nodejs_,
  and it might cause the installation to break. If this is the case, make a
  symbolic link from _nodejs_ to _node_.

- **Important**: for Windows, [Git-Bash](https://superuser.com/questions/1053633/what-is-git-bash-for-windows-anyway) (installed with git) is the recommended
  command-prompt, and it can be opened as a normal program form the start menu.


## Install nodeGame stable

- Download the [nodeGame installer](https://nodegame.org/nodegame-installer.js) (right-click, save as)
- Open a terminal and navigate to the folder where you downloaded the installer
- Install nodeGame v5 with the command: `node nodegame-installer.js`
- Follow instructions on screen


### Installing alternative nodeGame versions

- Install the development version with the latest features: `node
  nodegame-installer.js @dev`
- Install nodeGame v4: `node nodegame-installer.js @v4`
- Install nodeGame v3: `node nodegame-installer.js @v3`

## Running Your First Game

NodeGame has an <a href="https://en.wikipedia.org/wiki/Ultimatum_game"
target="_blank">Ultimatum Game</a> included in the default
installation. To start the server and play the game follow these
steps:

1. Open a terminal and navigate to the `nodegame` folder (for Windows, Git-Bash is the recommended command-prompt)
2. Start the server with the command: `node launcher.js`
3. While the server is running, open one browser tab pointing to: `localhost:8080`
4. Open more tabs, click "Play with Bots", or manually launch a bot connecting
   to: `localhost:8080/ultimatum/?clientType=autoplay`
5. Check the monitor interface: `localhost:8080/ultimatum/monitor`


## Next Topics

Next:
[Create a new game in nodeGame](Create-New-Game-v5)
