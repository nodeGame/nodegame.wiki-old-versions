- status: complete
- version: 2.x

## Top New features in nodeGame 2.x

1. **Game Levels**: divide a game in multiple parts, each with a
separate waiting room and game sequence.

2. **Enhanced Waiting Room**: new features and criteria for
dispatching game rooms, such as: title blinks and a sound is played to
alert player of the start of a game; the start of a game can be
set in the future; clients are pinged one more time before move them
to the new game room; and more.

3. **Matcher**: the Matcher class allows one to creates matches of
players according to predefined algorithms. At the moment are
available: *round-robin* (perfect stranger matching) and *random*.

4. **Enhanced disconnection handler**: most of the complexity is now
moved in the background and a default reconnection handler is added to
every logic.

5. **pushClients feature**: for selected steps, it is possible to
enforce a time limit on clients and push them to the next step, even
if a recoverable error has occurred on the remote machine.

6. **New Window commands**: additional commands to interact with the
DOM and the browser's window, such as `W.setInnerHTML`,
`W.searchReplace`, `W.onFocusIn`, `W.onFocusOut`,
`W.disableBackButton`, etc.

7. **New Step-Properties:** such as: `init`, `exit`, `timeup`, `frame`,
`widget`, `reconnect`, and more.

8. **Widget-Steps**: A widget can take care of handling a whole
step. No code needed from developer!

9. **New Widgets**: such as: `DoneButton`, `ChoiceTable`, `ChoiceGroup`,
`ChoiceManager`, `MoodGauge`, `SVOGauge`.

10. **Enhanced Requirements Room**: new features added to measure
connection speed, cookie support, etc.

11. **New options on launcher file**: such as: automatic rebuild of
selected nodeGame components (for developers), and support for SSL.


## Next Topics

Next: [Migrating to v.2](Migrating-To-v2)
