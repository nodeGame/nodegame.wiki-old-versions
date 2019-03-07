- status: complete
- version: 2.x

## Migrating to nodeGame 2.x

nodeGame version 2.x introduced only minor changes that are backward
incompatible. They are listed here:

1. `node.game.timer` is now a default object, containing the default
timer of the game. If your game already defined an object under the
same name (e.g. the VisualTimer object), it should be renamed or the
game might not run properly.

2. `settings.TIMER` in file `game.settings.js` is now a default object
containing the `timer` property of all the steps of the game. If your
setting file is already using a property with the same, it should be
renamed or the game might not run properly.

3. `settings.WAIT_TIME` in file `game.settings.js` is now the default
waiting time in case of disconnection of a player (in
milliseconds). If your setting file is already using a property with
the same, it should be renamed or the game might not run properly.

4. The following step-properties are now used by nodeGame: `init`,
`exit`, `frame`, `widget`, `timeup`, `reconnect`, `pushClients`. If
your game had already an step-property with the same, it should be
renamed or the game might not run properly.

5. The settings file for the waiting room `waitroom.settings.js`
must have option `EXECUTION_MODE = 'WAIT_FOR_N_PLAYERS'` defined.

### Recommended adjustments

Some files that were inside the game folder have now been moved into
the server. If you have not customized any of the following files, you
should delete them, so that the server will use its internal functions
instead:

- requirements/requirements.room.js
- requirements/requirements.js
- waitroom/waitroom.js

Dependency `JSUS` has been updated, and method
`JSUS.mkdirSyncRecursive` (as well as other file system related
methods) has been discontinued. If you were use that method, require
the `fs-extra` module, and call `fs.mkdirsSync`.


## Next Topics

Next: [Start the Tutorial v.2](Getting-Started-v2)
