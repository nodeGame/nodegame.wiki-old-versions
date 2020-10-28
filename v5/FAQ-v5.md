- status: in progress
- version: 5.x

## Frequently Asked Questions

**General Questions**

1. [What is nodeGame?](#q_whatis)
2. [What are nodeGame's strengths and weaknesses?](#q_strength_weak)
3. [Which languages do I need to know to create games in
nodeGame?](#q_language)
4. [What is the learning curve for nodeGame?](#q_learning)
5. [How much do I need to code for running a game/experiment?](#q_howmuch)
6. [In a nutshell, how do I implement a game in nodeGame?](#q_nutshellgame)

**Installation**

1. [I tried to install nodeGame, but I got an xcrun error: `xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun`. What can I do?](#q_xcrun)


**Technical Questions**

1. [What is the difference between `node.say` and `node.emit`?](#q_emit)
2. [Why do I get the message "_Error: EMFILE, too many open files_"?](#q_emfile)
3. [How can I get the address of the page currently loaded in the
iframe?](#q_iframe)
4. [I have updated the default game template, how do I see if my
changes are working?](#q_see_changes)
5. [How do I integrate libraries such as JQuery or Raphael.js in nodeGame?](#q_raphaeljs)
6. [How do I define a global variable?](#q_globalvariables)
7. [How do I debug nodeGame?](#q_debug)
8. [I enabled authorization and I got this error: `GameLoader.loadAuthDir: channel "XXX": auth is enabled, but file "channel.credentials.js"/"channel.secret.js" not found. Check the channel/ directory.` What can I do?](#q_secret)



### General Questions


<a name="q_whatis"></a>
1. **What is nodeGame?**

NodeGame is a free, open source JavaScript/HTML5 framework for
conducting synchronous experiments online and in the lab directly in
the browser window. 

It is specifically designed to support behavioral research along three
dimensions:

 - larger group sizes,
 - real-time (but also discrete time) experiments,
 - batches of simultaneous experiments. 
 
nodeGame has a modular source code, and defines an API (Application
Programming Interface) through which experimenters can create new
strategic environments and configure the platform. 

<a name="q_strength_weak"></a>
2. **What are nodeGame's strengths and weaknesses?**

NodeGame is particularly suited for performing synchronous
interactive behavioral economics experiments online or in the
laboratory. 

nodeGame is designed to scale up to hundreds of simultaneous
players, and each new release is carefully tested to meet the
standard.

nodeGame can run on a great variety of devices, from desktop computers
to laptops, smartphones, and tablets, and no installation is needed on
any client.

nodeGame can run multiple experiments at the same time in separate
channels, and the same game can have different entry points. Each game
has an integrated waiting room which controls when and how a new game
session is dispatched, and a monitor interface showing the current
state of the game and allowing the experimenter to interact with the
participants.

nodeGame offers easy authorization rules and browser's technical
requirement checks, which are performed _before_ letting a new client
entering the experiment.

At the moment, nodeGame does not offer support for 2D or 3D graphics.

nodeGame does not provide recruitment of participants for your
games/experiments. However, if you recruit participants from
[Amazon Mechanical Turk](http://mturk.com), you can use the
[nodegame-mturk](https://github.com/nodeGame/nodegame-mturk) module to
handle payments, qualifications, bonuses, etc.

Furthermore, if you are working at
[ETH Zurich](https://www.ethz.ch/en.html), you can also try the
[descil-mturk](https://github.com/nodeGame/descil-mturk) module to
integrate your game with the [ETH DeSciL](http://www.descil.ethz.ch/)
Mturk platform, which can handle recruitment and payment of
participants for a fee. If interested please contact the
administrative staff of DeSciL.

<a name="q_language"></a>
3. **Which languages do I need to know to create games in
nodeGame?**

You need low/medium knowledge of the
[JavaScript](http://javascript.info/) language, and you need to
understand how to create HTML pages.

Knowing [Node.JS](https://nodejs.org/en/) specific syntax will help
you to extend the platform or to create more complex games, but it is
not a requirement. 

To create rich user interfaces, user-interface frameworks such as
[jQuery](https://jquery.com/) or [Bootstrap](http://getbootstrap.com/)
can help.

Finally, if you want to dynamically build the HTML you might look into
the [Pug](https://pugjs.org/) templating language.

<a name="q_learning"></a>
3. **What is the learning curve?**

It really depends on your knowledge of JavaScript. For a developer
familiar with other languages, but with little knowledge of
JavaScript, it might take 2-3 days to understand the
framework. Once a developer is familiar with the framework, it
usually takes a couple of days to come with a testable prototype of
a game/experiment.
   
<a name="q_howmuch"></a>
4. **How much do I need to code for running a game/experiment?**

NodeGame is a programming framework, so programming is required. A
[command line utility](https://github.com/nodeGame/nodegame-generator)
is provided to generate a game template that can be modified and
adapted to your needs.

The following resources are provided by the framework:

- Waiting rooms
- Internal database
- Synchronization rules
- Timers
- Pausing / Resuming game
- Page customization (e.g. disable right click, prompt on leave
messages, waiting screens, etc.)
- Widgets (visualize timers, rounds number, language selector,
debug information, chat, etc.)
- Several event handlers (on disconnect, on entering a new step,
etc.)
- Authorization rules
- Requirements rules
- Game levels

The following resources must be coded by the developer:

- The HTML pages
- The game sequence
- The interaction among players

The following might be coded by the developer:

- What actions to take when a disconnected player
  disconnects/reconnects
  

<a name="q_nutshellgame"></a>
5. **In a nutshell, how do I implement a game in nodeGame?** 

You can start off using the
[generator](https://github.com/nodeGame/nodegame-generator) module
(Windows users read [here](Game-Basics-v5)). In your Terminal window,
go to the nodegame folder and type:

```
cd bin
node nodegame create-game MYGAME
```

Follow the instructions on screen and enter the information required
after prompted (folder path, default author name, default email). 

_Note_: MYGAME is the name of the project, which can be customized. A
folder named MYGAME will be created inside `nodegame/games/` with all
the subfolders that a nodegame experiment requires.

Your experiment will run following the sequence of steps defined in
file `game.stages.js` inside the folder `nodegame/games/MYGAME/game/`.

The way in which the steps of your experiment are reflected in the
code is as follows. The basic idea is that, in each step, all players
(and the server as well) perform certain actions and respond to other
player's actions. What these actions are and how to respond to them
depends on the step the experiment is in. What a player (and the
server) can do in each step is determined by the code inside:

```
stager.extendStep('name-of-step', { 
 // CODE HERE
)}; 
```

For browsers (clients), steps are defined in file _player.js_, inside
the folder `nodegame/games/MYGAME/game/client_types/`.

For logic (server), steps are defined in file `logic.js`, which is
inside the same folder as before.


### Installation


<a name="q_xcrun"></a>
1. **I tried to install nodeGame, but I got an xcrun error, what can I
do?** 

This error can occur if you are using Max OS X El Capitan or
Sierra and git is not properly installed.

First, check if you have [git](https://git-scm.com/) installed. Then,
in a Terminal window, try running:

```
git --version 
```

If you get something like:

```
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools),
missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
```

try using the following code to fix the problem

```
xcode-select --install
```


### Technical Questions

<a name="q_emit"></a>
1. **What is the difference between `node.say` and `node.emit` ?**

`node.say` sends a _message_ to a _remote_ recipient, while
`node.emit` emits a _local event_.

```javascript
// Information we want to send along.
var info = { foo: 'bar' };

// Pass the information locally
// (same client, different parts of application).
node.emit('local_event', info);

// Pass the information remotely
// (different clients, encapsulate info into a game msg).
node.say('remote_event', 'CLIENT_ID', info);
```

Event listeners `node.on` (for local events) and `node.on.data` (for
incoming messages) must be registered before the event is
fired/sent.

```javascript
node.on('local_event', function(info) {
   console.log(info.foo); // 'bar';
});
node.on.data('remote_event', function(msg) {
   console.log('Received msg from', msg.from); // '1234...';
   console.log('Msg contains data', msg.data.foo); // 'bar';
});
```

<a name="q_emfile"></a>
2. **Why do I get the message "_Error: EMFILE, too many open files_" ?**

   It can happens when the number of simultaneous connections is very
   high. The actual limit depends on the settings of your operating
   systems. On Linux/Mac systems it can be bypassed with the command:

       $ ulimit -n 2048

   More information and solutions available here:
   
   - http://blog.loadimpact.com/2013/03/19/know-your-node-js/
   - http://tgriff3.com/post/44864365776/fixing-error-emfile-too-many-open-files-in-node-js
   - http://stackoverflow.com/questions/8965606/node-and-error-emfile-too-many-open-files
   
<a name="q_iframe"></a>
3. **How can I get the address of the page currently loaded in the
iframe?**

Open the browser's javascript console and type: 

```
W.getFrameWindow().location.href
```

to see the full path, or simply:

```
W.unprocessedUri
```

to see what parameter was passed to the load-frame method.

<a name="q_see_changes"></a>
4. **I have updated the default game template, how do I see if my
changes are working?**

If you are eager to see if your changes to an experiment are working,
you should open a Terminal, go into the nodegame folder and type the
following:

```
node launcher 
```

You should see something like 

```
Requirements room created: ultimatum
Requirements room created: name-of-your-experiment 
```

_Note_: there will be a room for each experiment in the
`nodegame/games/` folder.

Now, open two (or as many as necessary) browser windows and in each
one of them put the following url:
`http://localhost:8080/name-of-your-experiment/`

_Important note_: If you want to see any further change to any of your
.js files reflected in the game behavior, you should stop nodegame
(press Ctrl+c on your Terminal) and run it again.

_Note_: If you make changes inside an html page, you can see its
effects by refreshing the browser without restarting nodegame.

<a name="q_raphaeljs"></a>
5. **How do I integrate libraries such as JQuery and Raphael.js in
nodeGame?**

Sometimes you need to load a special library in a particular step of
your game, for example
[Raphael.js](http://dmitrybaranovskiy.github.io/raphael/) or
[JQuery](https://jquery.com/). Frame loading in nodeGame makes use of
[iframes](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe),
therefore such libraries are normally not immediately accessible from
the `player.js` [client type](Client-Types-v5). Two solutions are available.

**Solution 1**

You could implement communication between the IFRAME and player.js
emitting events in nodeGame. So, inside the html iframe you should add
something like:

```javascript
<script>
window.onload = function() {
    var node = parent.node;
    node.on('mySettings', function(settings) {
        // HERE YOUR JavaScript + Raphael.js CODE
    });
}
</script>
```

Inside your `player.js` file you can send information to Raphael
using:

```javascript
node.emit('mySettings', MESSAGE);
```

_Comment_: You exchange _local_ information from the `player.js` to
the script in the html page (and vice versa) using `node.emit`. You
exchange _remote_ information from the logic to a player using
`node.say`:

```javascript
node.say('event', player_id, message);
```

and from the player to the logic using:

```javascript
node.say('event');
// or
node.say('event', 'SERVER', message);
```

_Multi-steps stages_

`node.on` creates an event listener that is bound to the **step** in
which it is created (for details see the
[Event Listeners page](Event-Listeners-v5)). So after the step is
finished, it is destroyed. If the HTTP server (or the browser) caches
the HTML page, the script inside the page will not be reloaded at each
new step, and therefore the event listener is not readded upon
starting the new step.

To avoid this problem, you can register the event listener on a higher
level. Instead of `node.on(...)` you use `node.events.stage.on(...)`,
and it will stay active for all the steps of the same stage. Now you
just need to pay attention that, in case the page is reloaded in some
browser, the script is not executed twice, and so the listener is also
added twice. See the code snippet below:

```javascript
<script>
window.onload = function() {
    var node = parent.node;
    
    // If the page is not cached, we exit 
    // to not add the event-listener twice.
    if ('undefined' !== typeof window.mySettingsCb) return;

    window.mySettingsCb = function(settings) {
        // HERE YOUR JavaScript + Raphael.js CODE
    };

    // This event listener is register on a higher level,
    // and will stay active for all the steps in the same stage.
    node.events.stage.on('mySettings', window.mySettingsCb);
}
</script>
```

**Solution 2**

If the functions you need to access are defined as properties of the
window object of the IFRAME (as in the code snippet at the end of
Solution 1), you can invoke them directly from `player.js`.

```javascript
var frameWindow = W.getFrameWindow(); 
frameWindow.mySettingsCb(...);
```

<a name="q_globalvariables"></a>
6. **How do I define a global variable?**

If you want to create global variables, you can include them in the
[game.settings.js](Game-Basics-v5) file. 

To access the content of a global variable defined in
game.settings.js, say `myGlobalVariable`, you can do the following:

- inside `logic.js`, you refer to the variable as
`settings.myGlobalVariable`;
- inside `player.js`, you can access it via
`node.game.settings.myGlobalVariable`.

_Important_: once the settings are sent to the clients, modifications
to global variables (both in player and in logic) are not propagated
and remain local.


<a name="q_debug"></a>
7. **How do I debug nodeGame?**

If you experience an unexpected behavior in your game, there are two
possible sources of errors: client and server.

To debug _client-side_ errors:

- Operate in client **debug** mode, if there is an error the client
  will crash immediately (**not** recommended in
  [production](Going-Live-v5)). Set debug = TRUE in `player.js` or
  `game.setup.js`.
- Open the Javascript console in your browser and looks for errors.
- The last error (if any) is available at `node.errorManager.lastError`
- Check server log file `messages.log`. Errors happened on the clients
  might be recorded here (depending on they type of error and the
  circumstances under which it happens, i.e. if it still possible to
  send messages).
- Check the monitor interface. Check if all the clients in the game
  room are at the game/step they should be.

To debug _server-side_ errors:

- Operate in server **debug** mode, if there is an error on the server
  it will crash immediately (**not** recommended in
  [production](Going-Live-v5)). Start nodegame with debug flag: ``node
  launcher -d``
- Then you can check the server logs:
    - `messages.log`: all messages exchanged to and from the server
      (might include remote errors too);
    - `channels.log` and `servernode.log`: high level errors, usually
      empty errors on your game, or if a file is missing, etc;
    - `clients.log`: the console.log output of any bot/phantom.js
      client used on the server.
      
<a name="q_secret"></a>
8. **I enabled authorization and I got this error: `GameLoader.loadAuthDir: channel "XXX": auth is enabled, but file "channel.credentials.js"/"channel.secret.js" not found. Check the channel/ directory.` What can I do?**

If you get this error you need to make sure you have the
`channel.secret.js` and file `channel.credentials.js` in the
_channel/_ directory of the game. These files are used to encrypt the
authorization tokens when a new client connect. These files are not
supplied by default for security reasons, as every new game needs to
personalize them. However, the channel directory normally includes two
`.sample` files that you can simply edit and rename. For more
information, refer to the
[channel configuration](Channel-Configuration-v5) page.
