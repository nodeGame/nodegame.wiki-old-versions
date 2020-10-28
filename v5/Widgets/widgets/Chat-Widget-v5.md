 - status : complete
 - version : 5.x

## Illustration
<figure>
  <img src="http://nodegame.org/images/wiki/v5/chat-widget-monitor.jpeg" alt="Illustration of the Chat widget in the monitor interface." width="300">
  <img src="http://nodegame.org/images/wiki/v5/chat-widget-client.jpeg" alt="Illustration of the Chat widget from a player's perspective." width="300">

  <br>
  <figcaption>Appearance of the Chat widget.</figcaption>
</figure>

## Introduction

It creates a configurable chat between participants or participants and
monitor. If the chat is minimized or closed a notification is sent to other
participants.

## Main Options

 - `receiverOnly`: If TRUE, no message can be sent, only received.
 - `chatEvent`: The event to fire when sending/receiving a message. Default:
     'CHAT'
 - `useSubmitButton`: If TRUE, a submit button is added, otherwise
      messages are sent by pressing ENTER. Default: TRUE on mobile devices
 - `useSubmitEnter`: If TRUE, pressing ENTER sends a msg. Default: TRUE
 - `storeMsgs`: If TRUE, a copy of every message is stored in a local db
 - `participants`: An array containing the ids of participants (cannot
      be empty); alternatively, the array may contain objects specifying incoming, outgoing, and display id for messages (see usecase below).
 - `initialMsg`: An object containing the initial message to be displayed as
      soon as the chat is opened (see usecase below).
 - `uncollapseOnMsg`: If TRUE, a minimized chat will automatically
      open when receiving a msg. Default: FALSE.
 - `printStartTime`: If TRUE, the initial time of the chat is
      printed at the beginning of the chat. Default: FALSE.
 - `printNames`: If TRUE, the names of the participants of the chat
      is printed at the beginning of the chat. Default: FALSE.
 - `showIsTyping`: If TRUE, a notification will appear when another   
      participant is typing. Default: TRUE.


## Usecase

```js
// Creates and appends a new Chat widget.
var root = document.body;
var chat = node.widgets.append('Chat', root, {
    participants: [ 'id1', 'id2', 'id3' ],
    initialMsg: { id: 'id1', msg: 'This is the first msg' },
    printStartTime: true,
    // Extra options available to all widgets.
    docked: true,
    collapsible: true,
    closable: true
});

// Creates and appends a new Chat widget that replaces the id of chat participant with a pseudonym.
var chat2 = node.widgets.append('Chat', root, {
    participants: [
        {
            // The id of the recipient of chat messages (msg.to).
            recipient: '987654321',
            // Optional. The id of recipient of the sender of chat messages,
            // if different from the recipient id. Default: equal to recipient.
            sender: '123456789',
            // Optional. The display name for incoming messages from sender.
            // Default: the recipient id.
            name: 'Alf'
        },
        {
            recipient: '12312312312',
            name: 'Malf'
        },
        {
            recipient: '456456456456',
            name: 'Ralf'
        }
    ],
    title: 'Mistery Chat',
    collapsible: true,
    closable: true,
    docked: true,
    printStartTime: true
});
```

## Texts

 - `outgoing`: Formats outgoing messages.
 - `incoming`: Formats incoming messages.
 - `quit`: Communicates that a participant has left the chat.
 - `noMoreParticipants`: Communicates that there are no participants left.
 - `collapse`: Communicates that a participant has collapsed or uncollapsed the
   chat.
 - `textareaPlaceholder`: The placeholder on the textarea.
 - `submitButton`: The text on the submit button.
 - `isTyping`: 'The text notifying that another participant "is typing...".

## Links


- [Source Code](https://github.com/nodeGame/nodegame-widgets/blob/master/widgets/Chat.js)
- [List of Widgets](Widgets-v5)
