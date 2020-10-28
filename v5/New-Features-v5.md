- status: complete
- version: 5.x

## New features in nodeGame 5.x

1. **Improved Monitor Interface**:

   - Client List is more compact, highlights currently selected room, displays
       total player and admin counts and room treatment.
   - All commands have been reorganized in small widgets that are responsive to
       the room currently selected.
   - Added a chat manager to easily start conversations with connected users.
   - Added WaitingRoom, RequirementRoom and ServerInfo viewers.
   - Added DebugWall at the bottom of the page.
   - Several visual improvements.
   - Performance improved by reducing the total number of messages exchanged.

2. **New Widgets**:

   - [Chat](Chat-Widget-v5): crates a configurable chat window (this is a
       complete rewrite of previous Chat widget).
   - [DebugWall](DebugWall-Widget-v5): displays all incoming and outgoing messages
       and nodegame logs in a configurable div.
   - [BoxSelector](BoxSelector-Widget-v5): displays a configurable button that
       if clicked opens a menu with a list of options to select from.
   - [DisconnectBox](DisconnectBox-Widget-v5): displays a button users can click
       to disconnect.
   - [BackButton](BackButton-Widget-v5): displays a button users can click
       to go back to the previous step.

3. **New Widgets features**:

   - Widgets can be closed, collapsed and restored via buttons on the header.
   - Widgets can be docked at the bottom of the page; docked widgets are
       automatically rotated if not enough space is available on page.
   - `#Widget.emit()` and `#Widget.on()`: widgets emit events: 'destroyed',
       'hidden', 'shown', 'hidden', 'enabled', 'disabled'.
   - `#Widget.panel` control if the panel is automatically added at creation of
       the widget

4. **New Waiting Room execution mode**: `WAIT_FOR_DISPATCH` simply waits
   indefinitely until a start command is received from the monitor interface
   (ideal for lab experiments).

5. **Support for array of recipients**: `#node.say` supports an array of
   recipients.

6. **Improved installer**: more reliability checks added and an easier way to
   install older version was introduced.

## Contributors

- Stefano Balietti <ste@nodegame.org>
- Ewen Wang <wang.yuxu@husky.neu.edu>

Special thanks to Edgar Andrade and Forough Poursabzi-Sangdeh for bug reporting.

## Next Topics

Next: [Migrating to v5](Migrating-To-v5)
