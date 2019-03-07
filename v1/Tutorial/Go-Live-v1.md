- status: complete
- version: 1.x
- follows from: [Miscellaneous](Miscellaneous-v1)

## Overview

Before you run your experiment (online or in the lab) you need to make
sure that the environment is properly configured for "production"
(that is when the application is accessed by real users). Production
settings differ from "development" settings, and below a checklist of
settings is provided.


### Go-Live checklist

1. **Disable `debug` mode.** When debug mode is enabled runtime errors
and exception will cause the server to stop. This is useful when
developing and debugging the application, but undesirable in
production mode. To disable debug mode edit file _game/game.setup.js_
and set `game.debug = false`.

2. **Set verbosity level of the clients.** Clients log a lot of useful
information while running a game. It is usually a good idea to hide
away this information from end-users for two reasons: to prevent
malicious usage of such information, and also because excessive
console printing can lower the game performance in older browser. To
control the level of output edit file _game/game.setup.js_ and set
`game.verbosity = 0` (only errors and warnings will be printed).

3. **Setup the UI of the browser.** Depending if you are running the
game in the lab or online the recommended settings differ. In the lab,
ideally you should try to run the browser in
[kiosk mode](https://en.wikipedia.org/wiki/Kiosk_software). Kiosk mode
is independent of nodeGame, and must be configured separately. If
kiosk mode is not available in your university lab, you should at
least block right-clicking _game/game.setup.js_
`game.window.disableRightClick = true` and display a message if a user
tries to close the browser `game.window.promptOnleave =
true`. However, it is never recommended to disable right click for
online games.

4. **Enable reconnection.** When reconnection is on, clients
refreshing the page (accidentally or on purpose) are put back into the
game room (instead of the waiting room). Reconnection requires
authorization to be correctly set to work, it is not necessary for lab
experiments, but still recommended. To enable it modify
_channel/channel.settings.js_ and set `enableReconnections` equal to
true. Depending on the design of your game, you might have to write a
custom reconnection handler.

5. **Add authorization rules.** When authorization is on, only one tab
per browser is allowed to connect to the game. Connecting a second tab
will cause disconnecting the previous one. To enable authorization
make sure you have valid `channel.credentials.js` and
`channel.secret.js` files (not the sample ones) inside the `channel/`
folder. Then in file `auth/auth.settings.js` set `enabled` equal to
true. Finally, create your codes in file `auth/auth.codes.js`. When
authorization is correctly setup, all players must connect through the
address `http://yourserver/yourgame/auth/id/password`, where id and
password are as you specified in your codes list. See the
[Game Advanced](Game-Advanced-v1) page for details.

6. **Add aliases.** If the address bar of the browser is visible (this
is normally the case online), the name of the game will be readable
there. It might be that the name is already indicative of the research
question at stake. For example, if the name of the game contains the
word "sharing," this might prime participants towards more cooperative
behavior. To avoid this, you can setup a neutral alias, such as "game"
or "experiment" that will be displayed instead of the actual game
name. Notice that the name of the pages and treatments are never
displayed in the address bar, so you don't have to worry about
those. To add one or more aliases edit the file
`channel/channel.settings.js` and type the name of the aliases inside
the `alias` field.

7. **Disable Channel Query Parameters.** Channel query parameters
(also called socket-io query parameters) are passed as extra
parameters along the url of the nodeGame server. They instruct the
nodeGame channel to setup the incoming connection in a given way, for
example defining the client type and connecting room. For details see
[here](https://github.com/nodeGame/nodegame/wiki/Game-Basics-v1#client_types).
Channel query parameters should be disabled when running experiments
online (remember you can still add computer-controlled players from
the monitor interface), while they might be kept on in the lab as long
the browser runs in kiosk mode (see point 3. of this list). To disable
channel query parameters edit the file `channel/channel.settings.js`
and add a `sioQuery = false` field.

8. **Set auto-push/disconnect feature.** _Experimental_. This feature
is only available in the development version of nodeGame, and it is
recommended for online games. If enabled, non-responsive clients will
be pinged and asked to advance to the next step. If still
unresponsive, clients will be force to disconnect. To enable add
`pushClients: true` to all the steps and/or stages of the game
sequence where you would like the logic to execute this feature. As
this option is still _experimental_, make sure to test the effects of
activating it before running your live session.

9. **Make sure have removed all `debugger` statements.** If you made
use of the Node.js debugger there is a good chance that you forgot a
debugger statement. To check for forgotten debugger statements you can
run the _grep_ program in your `game/` folder: `grep debugger ./
-R`. In Windows you must download an equivalent grep program, for
example [wingrep](http://www.wingrep.com/).

10. **Run jshint on your sources.** [JSHint](http://jshint.com/) is a
tool that helps to detect errors and potential problems in your
JavaScript code. No matter how good a programmer you (think) you are,
you will always benefit from using jshint. Install jshint globally
with the command `npm install -g jshint` (might need admin rights),
and then run jshint inside your `game/` folder: `jshint
--show-non-errors *`.


  
## Go Back to

* [Home](Home)

