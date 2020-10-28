- status: complete
- version: 5.x

## Will My Game Run Correctly in Every New Release of nodeGame?

All new releases of nodeGame follow [semantic versioning](https://www.geeksforgeeks.org/introduction-semantic-versioning/). Your game is guaranteed to run in any non-major new release. Consider the following examples of version changes:

- From _5.0.0_ to _5.0.1_ (patch): your game will run fine without changes.

- From _5.0.x or to _5.1.0_ (minor release): your game will run fine without changes, but minor UI modifications are possible, such as the default size of a font inside a widget. Experimental (undocumented) API might also change.

- From _5.x.y_ to _6.0.0_ (major release): you should test your game carefully and probably update some of its code. For every major release a [migration guide](Migrating-To-v5) is available highlighting all backward-incompatible changes and how to fix your code.

If your game fails to run after a minor or patch update, please report this unexpected behavior immediately.


## Determine If You Can Update nodeGame with Git

The fastest way to update nodeGame to the latest version is to use [Git](https://git-scm.com/). However, you can do so only in the development version of nodeGame.

To check if you can update nodeGame with Git, open a terminal and navigate to the nodeGame installation directory.

If the name of your installation directory ends with `-dev`, you will be able to use Git, otherwise you will need to run a new installation and copy over your games. Both methods are explained below.

## Updating nodeGame by Running a New Installation

Download the _latest_ version of the nodeGame installer and run it following [these instructions](https://nodegame.org/install.htm). If you wish, you may install the [_dev version_](Getting-Started-v5#installing-alternative-nodegame-versions) to enable future Git updates.

After the installation is finished, copy over the `games/` and `games_available/` folders inside the new nodeGame directory. Important! Check for hard links in the games folder still pointing to the old folder and, in that case, update them accordingly.

If you had special settings in the `conf/` directory (e.g., specifying the port), you will also need to manually copy over.

After you have verified that the new installation succeeded, you may delete the old version of nodeGame (in production environments you may also store a copy of the `log/` folder).

## Updating nodegame via Git

You can perform the update _manually_ or using the _"pull-all"_ script.

### Manual Git update

You need to first update the individual nodeGame components and then rebuild the client file.

#### Updating nodeGame components

Navigate to the `node_modules/` folder inside the nodeGame installation directory. Enter each of the following sub-directory:

- nodegame-client
- nodegame-server
- nodegame-window
- nodegame-widgets
- nodegame-requirements
- nodegame-game-template
- nodegame-monitor
- JSUS
- NDDB
- nodegame-generator
- nodegame-mturk
- nodegame-db
- nodegame-mongodb

and execute the following git command:

```
git pull
```
#### Rebuild the client file

When you have finished updating all the desired nodeGame components, you need to rebuild the client file `nodegame-full.js`. To do so, you need to start nodeGame with the build option (just the first restart):

```
node launcher.js -b window,widgets,JSUS,NDDB
```

There is no need to specify other components, because they are either automatically included in every new build or not sent to the client at all.

#### Partial update

If you wish, you can just update some components. For example, if you just updated the widgets, you just need to rebuild the widgets at the next restart.

```
node launcher.js -b widgets
```

#### Troubleshooting

The manual update will fail if you have made any changes to the source code inside a component. For instance, every new build (`-b option`) modifies the source code of one or more component. If the changes can be discarded, you can reset the individual components before pulling the new changes:

```
git reset --hard
git pull
```

If you want to keep your changes, you need to commit them first.

```
git add .
git commit -m "my important changes"
git pull
```
However, this may cause a conflict with the next update of nodeGame, which needs to be resolved manually.

### Update via "Pull-all" Script

This [script](https://raw.githubusercontent.com/nodeGame/nodegame/master/bin/legacy/pull-all.sh) automatizes the manual update process.

Right-click and save it inside the nodeGame installation directory.

Open a terminal (in Windows Git Bash) and execute:

```
chmod +x pull-all.sh
./pull-all.sh
```

Following completion of the update, you need to rebuild the client file following the procedure outlined above.

Notice: the script is very basic and does not handle failures, such as: git repositories not available, or source codes modified.
