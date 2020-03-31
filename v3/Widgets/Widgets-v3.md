- status: incomplete (missing some of the widget description pages)
- version: 3.x

Widgets are reusable user interface components that can be loaded
dynamically in a nodeGame game. They serve multiple purposes. For
example, they can display the values of internal timer objects, the
number of remaining rounds in a stage, the amount of monetary rewards
gained by a player so far, etc. They can also offer a chat window to
communicate between experimenter and participants, or simply display
debugging information during the development and testing of the
experiment.

## List of Available Widgets

- [Visual Timer](VisualTimer-Widget-v3): displays a countdown-timer.
- [Visual Round](VisualRound-Widget-v3): displays a the count of rounds,
  stages, and steps.
- [Choice Table](ChoiceTable-Widget-v3): creates a configurable table
  that reacts to user's clicks.
- [Choice Table Group](ChoiceTableGroup-Widget-v3): groups together and
  manage multiple choice tables.
- [Choice Manager](ChoiceManager-Widget-v3): groups together and
  manage multiple choice-type widgets.
- [Mood Gauge](MoodGauge-Widget-v3): displays an interface to measure
  mood.
- [SVO Gauge](SVOGauge-Widget-v3): displays an interface to measure
  Social Value Orientation (S.V.O.).
- [Done Button](DoneButton-Widget-v3): displays a configurable button
  that calls `node.done`.
- [Money Talks](MoneyTalks-Widget-v3): displays a box to visualize
  monetary earnings during a game.
- [Language Selector](LanguageSelector-Widget-v3): builds a language
  selector form.
- [Debug Info](DebugInfo-Widget-v3): displays information about the
  state of the node instance.
- [Visual Stage](VisualStage-Widget-v3): displays current, previous and
  next stage of the game.
  
### Available Widgets, but description still missing
  
- [Chat](Chat-Widget-v3): builds a chat interface with different
  communication options.
- [Chernoff Faces](ChernoffFaces-Widget-v3): builds a interface to create
  configurable Chernoff faces.
- [Feedback](ChernoffFaces-Widget-v3): builds a configurable feedback form.
- [Wall](Wall-Widget-v3): displays a short log of all exchanged messages.
- [D3](D3-Widget-v3): creates charts with the
  [D3 library](http://d3js.org/). Experimental!
 

## Next

- [Learn how to use the widgets](Using-Widgets-v3)
