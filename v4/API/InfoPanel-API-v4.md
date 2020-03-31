- status: complete
- version: 4.x

## Introduction

The _InfoPanel API_ is a subset of the [Window](Window-API-v4) API. The Info
Panel is a configurable extra panel at the top of the screen, normally placed
between header and main frame.

## Illustration

<figure>
  <img src="http://nodegame.org/images/wiki/infopanel.jpeg" alt="Illustration of the InfoPanel widget">
  <br>
  <figcaption>In the figure above, the InfoPanel is the area of between the
header and the main frame, here displayng the "Round Results." In the header,
you can see the InfoPanel toggle button labeled "History," which opens and
closes the InfoPanel.</figcaption>
</figure>

### Generate the Info Panel

- **W.generateInfoPanel()**

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
   // In this example, the InfoPanel is used to contain the history
   // of the game. A toggle button is added to the header to open
   // and close the InfoPanel.

   var infoPanel = W.generateInfoPanel();
   this.historyButton = infoPanel.createToggleButton('History');
   this.historyButton.disabled = true;
   
   this.historyDiv = document.createElement('div'); 
   this.historyDiv.innerHTML = '<h3>Game history</h3>';
   
   infoPanel.infoPanelDiv.appendChild(this.historyDiv);

   document.body.appendChild(this.historyButton);
```  


## Sources

- [Full source and documentation](http://nodegame.github.io/nodegame-window/docs/lib/InfoPanel.js.html)

