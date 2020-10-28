- status: complete
- version: 5.x
- follows from: [Widgets Operations](Widgets-Operations-v5)

## Widget options

When creating a new widget instance via `get` or `append`

```js
node.widgets.get('VisualTimer', options);
node.widgets.append('VisualTimer', root, options);
```

the following options are available to control:

**Core elements**

  - `title`: sets the title in the header of the widget. If FALSE, the header is
    removed. Default: the widget's title or en empty string.
  
  - `footer`: sets the content in the footer of the widget. If FALSE, the footer is
    removed. Default: FALSE.
    
  - `context`: sets the Bootstrap framework context for the widget (e.g.,
    'primary', 'danger', etc.').
 
  - `className`: adds a className to the widget's panelDiv element.

**Initial state**

  - `disabled`: if TRUE, the widget will start in a disabled state. Default:
    FALSE.
  
  - `highlighted`: if TRUE, the widget will start in a highlighted
    state. Default: FALSE.

  - `collapsed`: if TRUE, the widget will start in a collapsed state. Default:
    FALSE.

  - `hidden`: if TRUE, the widget will start in a collapsed state. Default:
    FALSE.

**Buttons in the header**

  - `closable`: if TRUE a button is added in th header to close (destroy) the
    widget.

  - `collapsible`: if TRUE a button is added in th header to collapse/uncollapse
    the widget.
       
  
  - `collapseTarget`: if specified, when collapsed, a widget is moved inside
    this element. Note: a shared collapseTarget element for all widgets can be
    specified via the global widgets setup.


**Other**

  - `docked`: if TRUE, the widget will be docked at the bottom of the page

  - `storeRef:` if FALSE, the widget is not added to `node.widgets.instances`
    and `node.widgets.lastAppended`. Default: TRUE. Note: cannot be FALSE if
    `docked` is TRUE.

  - `id`: user-defined id (added as `.id`).


Additionally, each widget is assigned a random unique id stored under the
(`.wid` property)

  - `wid`: random unique widget id.

## Next

- [See all widgets methods](Widgets-Methods-v5)
