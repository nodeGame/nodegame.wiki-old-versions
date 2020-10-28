- status: complete
- version: 5.x

## Introduction

The _InfoPanel API_ is a subset of the [Window](Window-API-v5) API. The Info
Panel is a configurable extra panel at the top of the screen, normally placed
between header and main frame.

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/infopanel.jpeg" alt="Illustration of the InfoPanel widget">
  <br><br>
  <figcaption>In the figure above, the InfoPanel is the area of between the
header and the main frame, here displayng the "Round Results." In the header,
you can see the InfoPanel toggle button labeled "History," which opens and
closes the InfoPanel.</figcaption>
</figure>

### Generate the Info Panel

- **W.generateInfoPanel(root, options, force)**: Appends the Info
  Panel to the page, adds a reference inside the `W` object, and
  returns it. All input parameters are optional. The options object is
  passed to the Info Panel constructor, and it accepts the following
  options (defaults):

     - 'className': a class name for the info panel div (''),
     - 'isVisible': if TRUE, the info panel is open immediately (false),
     - 'onStep:' an action ('clear', 'open', 'close') to perform every
       new step (null),
     - 'onStage:' an action ('clear', 'open', 'close') to perform
       every new stage (null).

#### API methods

- **InfoPanel.clear()**: Clears the content of the Info Panel

- **InfoPanel.destroy()**: Removes the Info Panel from the DOM and the internal
    references to it

- **InfoPanel.toggle()**: Toggles the visibility of the Info Panel

- **InfoPanel.getPanel()**: Returns the HTML element of the panel (div)

- **InfoPanel.open()**: Opens the Info Panel

- **InfoPanel.close()**: Closes the Info Panel

- **InfoPanel.createToggleButton(buttonLabel)**: Creates an HTML button with a
    listener to toggle the InfoPanel

#### Examples:

```javascript
   // Generate the info panel object.
   var infoPanel = W.generateInfoPanel();
   // The reference W.infoPanel is automatically created.

   // Generate the toggle button and append it to the header.
   this.historyButton = infoPanel.createToggleButton('History');
   W.getHeader().appendChild(this.historyButton);

   // Add a new div to the info panel.
   this.historyDiv = document.createElement('div');
   this.historyDiv.innerHTML = '<h3>Game history</h3>';
   infoPanel.infoPanelDiv.appendChild(this.historyDiv);
```


## Sources

- [Info Panel](http://nodegame.github.io/nodegame-window/docs/lib/InfoPanel.js.html)

