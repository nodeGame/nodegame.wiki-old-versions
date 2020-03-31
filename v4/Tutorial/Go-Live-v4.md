- status: complete
- version: 4.x
- follows from: [Logging](Logging-v4)

## Overview

Before you run your experiment (online or in the lab) you need to make
sure that the environment is properly configured for "production"
(that is when the application is accessed by real users). Production
settings differ from "development" settings, and below a checklist is
provided.


### Go-Live checklist

#### Channel Options

In file `channel/channel.settings.js`:

- **Hide the game name.** If the address bar of the browser is visible (as it is
usually the case), the name of the game will be readable there. It might be that
the name is already indicative of the research question at stake. For example,
if the name of the game contains the word "sharing," this might prime
participants towards more cooperative behavior. To avoid this, you can setup a
neutral alias, such as "game" or "experiment" that will be displayed instead of
the actual game name. Alternatively, you can set the channel of the game as the
"default" channel, and in such a case no name will appear at all (beware: when
you specify a default channel, other games might no longer be reachable). Notice
that the name of the pages and treatments are never displayed in the address
bar, so you don't have to worry about those. To add one or more aliases edit the
file and type the name of the aliases inside the `alias` field. To set the game
as the "default" channel, add `defaultChannel: true` (remember to update the
entry points, and that other games will not be available, see all
[implications](Channel-Configuration-v4)).

- **Enable reconnection.** When reconnection is on, clients that disconnects and
then reconnects are put back into their old game room (instead of the waiting
room). To enable reconnection set `enableReconnections` equal to true. Depending
on the design of your game, you might have to write a custom [reconnection
handler](Handling-Disconnections-v4). Reconnection also requires authorization
to be enabled and properly configured (see below).

- **Disable Channel Query Parameters.** Channel query parameters (also called
socket-io query parameters) are passed as extra parameters along the url of the
nodeGame server. They instruct a nodeGame channel to setup the incoming
connection in a given way, for example defining [the client type and connecting
room](Channel-Configuration-v4#sioquery).  Channel query parameters should be
disabled when running experiments online (remember you can still add
computer-controlled players from the monitor interface), while they might be
kept on in the lab as long the browser runs in kiosk mode (see point 3. of this
list). To disable channel query parameters set `sioQuery = false`.

- **Change the default admin endpoint.** If you are running an online game, you
should change the administrative endpoint from `/admin` to something else; to do
so set `adminServer = '/myendpoint'`. See the [Game Advanced](Game-Advanced-v4)
page for details.


#### Setup Options

In file `game/game.setup.js`:

- **Disable `debug` mode.** When debug mode is enabled runtime errors will
cause the server to stop immediately. This is useful when developing and
debugging the application, but undesirable in production mode. To disable debug
mode set `setup.debug = false` (assuming that the name of your returning
variable is `setup`).

- **Set the verbosity level of the clients.** Clients [log](Logging-v4) a lot
of information while running a game. It is usually a good idea to hide away this
information from end-users for two reasons: to prevent malicious usage of such
information, and also because excessive console printing can lower the game
performance in older browser. To log errors and warnings set: `setup.verbosity =
0` (assuming that the name of the returning variable is `setup`).


- **Setup the User Interface (UI) of the browser.** Depending if you
are running the game in the lab or online the recommended settings
differ. Generally, it is not recommended to restrict the browser's features for
online games. In the lab, ideally you should try to run the browser in
[kiosk mode](https://en.wikipedia.org/wiki/Kiosk_software). Kiosk mode
is independent of nodeGame, and must be configured separately. If
kiosk mode is not available in your university lab, you can restrict
some of the UI functionlities by editing `_game/game.setup.js_`:
    
    ```javascript
    // Assuming `setup` is the name of the returning variable.
    setup.window = {
        // Block right-clicking.
        disableRightClick: true,
        // Display a message if a user tries to close the browser.
        promptOnleave: true,
        // Disable the back button.    
        disableBackButton: true
    };
    ```

    Furthermore, is a good idea to know if a player is switching
    between tabs. In the init function of your game you can add
    something like:

    ```javascript
    // This is called when a new tab is open (but not on a new window).
    W.onFocusOut(function() {
        // For example, if this is a lab experimenter:
        alert("Go back to the experiment's tab. " +
              "If unsure how to do it contact the experimenter.");
        // Or just log it (logic must listen to the message).
        node.say('tabswitch');
        // Or count them and log them at the end.
        node.game.tabswitches++; // (must be initialized before)
    });
    
    // This is called when the focus returns on the experiment's tab.
    W.onFocusIn(function() {
        // Log it, or do nothing.
    });
    ```

    Alternatively, if you are running a lab experiment on the Chrome
    browser, you can make use of the
    [Kiosk extension](https://chrome.google.com/webstore/detail/kiosk/afhcomalholahplbjhnmahkoekoijban?hl=en). Notice
    that this extension only simulates a real kiosk mode (e.g. alt+Tab
    will let the user switch between applications, etc.).

#### Authorization Rules

In file `auth/auth.settings.js`:

- **Specify authorization rules.** When authorization is on, only one tab per
browser is allowed to connect to the game. Connecting a second tab will
disconnect the previous one. To enable authorization set `enabled` equal to true
and specify an [authorization mode](Authorization-Rules-v4). When authorization
is enabled, all players must connect through the address
`http://yourserver/yourgame/auth/id/password`, where id and password are as you
specified in your codes list. Notice that authorization rules are not required
for lab experiments, but there some advantages in using them. In the unlikely
event of a technical failure of a computer, a disconnection will be detected. If
you are running your experiment without authorization there is no way to recover
this participant. If authorization is on, you can open a new browser window on
any other computer to the address `localhost:8080/MYGAME/auth/PLAYER_ID` and
then the player is back where he/she left.

Make sure you have valid `channel.credentials.js` and `channel.secret.js` files
(not the sample ones) inside the `channel/` folder.

#### Your Game Code

- **Set auto-push/disconnect feature.** This feature is recommended
for online games. If enabled, non-responsive clients will be pinged
and asked to advance to the next step. If still unresponsive, clients
are forced to disconnect. To enable, add `pushClients: true` to all
the steps and/or stages of the game sequence where you would like the
logic to execute this feature. Read about
[custom pushClients settings](PushClients-v4).


- **Clean up your code.** Make sure to remove all `debugger`
statements. If you made use of the Node.js debugger there is a good
chance that you forgot a debugger statement. To check for forgotten
debugger statements you can run the _grep_ program in your `game/`
folder: `grep debugger ./ -R`. In Windows you must download an
equivalent grep program, for example
[wingrep](http://www.wingrep.com/). Moreover, you should run jshint on
your sources. [JSHint](http://jshint.com/) is a tool that helps to
detect errors and potential problems in your JavaScript code. No
matter how good a programmer you (think) you are, you will always
benefit from using jshint. Install jshint globally with the command
`npm install -g jshint` (might need admin rights), and then run jshint
inside your `game/` folder: `jshint --show-non-errors *`.

### And then...

Make sure you re-run your game at least once after having 
edited your sources with production settings...and you are good to go!
  
## Go Back to

* [Home](Home)

