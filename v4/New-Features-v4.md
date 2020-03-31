- status: complete
- version: 4.x

## Top 10 New features in nodeGame 4.x 

1. **Home Page**: all available games are displayed in a grid of responsive
cards in the home page. Customize display of games specifying the `card`
property in the
[package.json](https://github.com/nodeGame/nodegame/wiki/Game-Advanced-v4#package_json_card)
file inside the game folder.

2. **Overhauled User Interface**: New game interface with slim header
occupying less space.

3. **InfoPanel**: customizable [drop-down panel](InfoPanel-v4) to easily display and hide
information under the header. 

4. **New Widgets and New Widgets Features**:

   _New Widgets_:

   - [EndScreen](EndScreen-Widget-v4): displays information such as earnings, exit code, feedback and email forms.
   - [Feedback](Feedback-Widget-v4): collect feedback (compatible as Choice-Like widget).
   - [EmailForm](EmailForm-Widget-v4)`: widget (compatible as Choice-Like widget).

   _New Widget features_:

   - `#Widget.addFrame()` and `#Widget.removeFrame()` display/remove the border and    
     margins around the bodyDiv element of the widget. This way it is easier to
     compose nested widgets. `frame` can be passed as an option to the 
     `#node.widgets.append()` method and if FALSE it behaves like
     `#Widget.removeFrame()`.
   - All texts and sounds used by widgets can be easily modified via `#Widget.setTexts() `#Widget.setSounds()` 
   - New WaitingRoom widget options:
       - `PAGE_TITLE` sets page title,
       - `ALLOW_PLAY_WITH_BOTS` adds a button to the waiting room to fill empty slots with bot players and start the game
       - `TEXTS.blinkTitle` and `SOUNDS.dispatch` sound can be disabled passing `false`.


5. **Wait Screen Countdown**: a countdown is displayed in the gray wait screen
after a player is done with current step. This way, he or she knows exactly what
is the maximum waiting time.

6. **Compute User Bonus**: `#GameRoom.computeBonus()` can be used to save a
bonus file and send earned bonus to each player. It integrates with widget
`EndScreen` for easy visualization for end of game parameters.

7. **New Channel Settings**:

   _Data Directory_:

   - `roomOwnDataDir`: controls if every new room gets a nested data dir,
   - `roomCounter`: sets the initial room counter,
   - `roomCounterChars`: adds a padding in front of the room counter.

   _noAuth Token_:

   - `noAuthToken`: even when authorization is disabled it is possible to set an
     authorization cookie to avoid multiple connections from same browser, making
     also reconnections possible.


8. **Streamlined Client Types**: Declaration of client types is a bit easier. No
need to return a client type object, it is now automatically assembled from the
stager and and the setup object.

9. **Treatment Settings in Views**: Views receive the correct settings object
depending on treatment. However, if authorization is off, it requires enabling
option noAuthCookie in the channel configuration.

10. **New Matcher Features**: two additional options: `canMatchSameRole`,
`fixedRoles`.


### Other improvements...

- Updated `connectBot` method with more options.
- Step property `frame` option can now be also a callback function.
- Improved logging stream.
- Updated monitor interface: kick player and connect bots buttons.
- Using SASS for nodegame.css.
- Bug fixes.


## Contributors

- Stefano Balietti <ste@nodegame.org>
- Ewen Wang <wang.yuxu@husky.neu.edu>
- Michael Yang <yangmichael@nyu.edu>
- Don Morrison <dfm2@cmu.edu>

## Next Topics

Next: [Migrating to v.4](Migrating-To-v4)
