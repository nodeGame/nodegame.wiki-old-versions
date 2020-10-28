- status: complete
- version: 5.x
- follows from: [Getting Started](Getting-Started-v5)

## Create a New Game

Inside your nodegame installation folder type

```
   cd bin
   node nodegame create-game gamename
```

This will create a new game folder named _gamename_ under the nodeGame installation folder in the `games_available/` folder with a link inside the `games/` folder. The new directory will contain all the necessary files that you can then modify to suit your needs.

**Note**: The difference between games/folder and games\_available/ folder is the following:

- Only the games in the `games/` folder are loaded by the server. When you have many games, but only some should be active (that is: playable), you may keep the games you don't want to be active in `games_available`. This terminology derives from how the [Apache server](https://httpd.apache.org/) handles multiple web sites. If it sounds confusing to you, you may think of games as "games\_enabled".

## Next

* [Learn about the files and folders inside a new game](Game-Basics-v5)
