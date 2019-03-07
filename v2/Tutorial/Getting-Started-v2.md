- status: complete
- version: 2.x

## Quick start

- Download version 4.x (or higher) of [node.js](http://nodejs.org) for
  your platform
- Download and run the install scripts for
  [Mac/Linux](https://raw.githubusercontent.com/nodeGame/nodegame/master/bin/install.stable.sh)
  or
  [Windows](https://raw.githubusercontent.com/nodeGame/nodegame/master/bin/install.stable.cmd)
- Open a terminal and browse to the `nodegame/` folder
- Start the server with the command: `node launcher.js`
- Open two or more browser tabs pointing to `localhost:8080/ultimatum`
- Alternatively, launch a bot connecting to: `localhost:8080/ultimatum/?clientType=autoplay`
- Open one browser tab pointing to `localhost:8080/ultimatum/monitor/`

## Installation

First, make sure to have a recent version of
[node.js](http://nodejs.org) installed on your
computer. **Important**: some Linux distributions install an
executable named _nodejs_, and it might cause the installation to
break. If this is the case, then make a symbolic link from _nodejs_ to
_node_.

### Stable version

 1. Choose the installer for your operating system:
 [Mac/Linux](https://raw.githubusercontent.com/nodeGame/nodegame/master/bin/install.stable.sh)
 or
 [Windows](https://raw.githubusercontent.com/nodeGame/nodegame/master/bin/install.stable.cmd)
 2. Right click and 'save as' the installer into your computer. If you
 are using Windows make sure to save the installer as a normal file,
 and not as a text file.
 3. If you are using Mac/Linux, open a terminal and type the following commands: 
 
    ```
    chmod +x install.stable.sh
    ./install.stable.sh
    ```
 4. If you are in Windows, open a terminal and type the following
 commands:
 
    ```
    start install.stable.cmd
    ```

### Development version

 1. Make sure you have a recent version of
 [git](http://www.git-scm.com) installed on your computer
 2. Choose the installer for your operating system: 
 [Mac/Linux](https://raw.githubusercontent.com/nodeGame/nodegame/master/bin/install.latest.sh)
 or
 [Windows](https://raw.githubusercontent.com/nodeGame/nodegame/master/bin/install.latest.cmd)
 3. Right click and 'save as' the installer into your computer. If you
are using Windows make sure to save the installer as a normal file,
and not as a text file.
 4. If you are using Mac/Linux, open a terminal
and type the following commands:

    ```
    chmod +x install.latest.sh
    ./install.latest.sh
    ```
 4. If you are in Windows, open a terminal and type the following commands: 
    
    ```
    start install.latest.cmd
    ```

## Running the demo game

1. After the installation is complete a folder named `nodegame/` is created
2. Open a terminal and browse to the nodegame/ folder
3. Start the server with the command: node launcher.js
4. Open two or more browser tabs pointing to `localhost:8080/ultimatum`
5. Alternatively, launch a bot connecting to: `localhost:8080/ultimatum/?clientType=autoplay`
6. Open a browser tab pointing to: `localhost:8080/ultimatum/monitor`


## Next Topics

Next: [Learn about how to create new games in nodeGame](Game-basics-v2)
