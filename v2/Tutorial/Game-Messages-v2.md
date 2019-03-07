- status: complete
- version: 2.x

## Game Message

A game message is the fundamental data unit exchanged by `node` instances.

It is is composed of the following fields:

```javascript
// A random generated unique id.
id: 735605,

// The session id as communicated by the server
// upon connection. Messages with a wrong session
// id will not be delivered.
session: "2614842012990266",

// The id of the client were the message was generated.
from: "435250987065956",

// The action field: "say", "set", or "get".
action: "say",

// The target field (see below for all accepted values).
target: "DATA",

// The client id of the recipient of the message.
to: "ID"

// The game stage during which the message was created.
stage: GameStage {
     stage: 1,
     step: 1,
     round: 1
},

// An optional field containing
data: Array[4],

// The timestamp of the creation.
created: "8-11-2014 13:28:47 898",

// A flag indicating whether this messages was forwarded.
forward: 0,

// The priority of the message. High priority
// messages are fired also when the game is paused.
priority: 0,

// A flag indicating whether an acknowledgment of
// the reception is requested. (NOT USED AT THE MOMENT).
reliable: 1
```

### The TO Field

The **TO** field is a string with the client id or client id alias of the
receiver.
It can also accept the following special values:

- `ALL`: Delivers to all clients in all channels.
- `CHANNEL`: Delivers to all clients connected to the same channel as the
  sender.
- `ROOM`: Delivers to all clients connected to the same room as the sender.
- `CHANNEL_channel_name`: Delivers to all clients in the channel with name
  `channel_name`.
- `ROOM_room_id`: Delivers to all clients in the room with id `room_id`.

The TO field can be modified by the server if the sender does not have the
permissions to send a message to a particular recipient or set of recipients.

### The TARGET Field

The **TARGET** field specifies the semantic meaning of the message.
It can contain the following values:

- `HI`: Welcome message by the server upon successful connection.
- `DATA`: Generic data message.
- `REDIRECT`: Message will redirect the client to a new URL.
- `ALERT`: An alert message will be displayed in the client window.
- `PLAYER_UPDATE`: Message containing an update from a player (e.g. change of
  state or stage levels).
- `LANG`: Message will set the default language of the client.
- `SETUP`: Message will setup one or more features of the client.
- `GAMECOMMAND`: Message will execute a game command: "start", "stop", etc.
- `SERVERCOMMAND`: A client is invoking a server command.
- `PCONNECT`: A new player connected in the same room.
- `PDISCONNECT`: A player disconnected from the same room.
- `PRECONNECT`: A previously disconnected player reconnected.
- `MCONNECT`: An administrator client (monitor) connected in the same room.
- `MDISCONNECT`: An administrator client (monitor) disconnected from the same
  room.
- `PRECONNECT`: A previously disconnected administrator (monitor) reconnected.
- `PLIST`: Message containing an updated player list.
- `MLIST`: Message containing an updated admin (monitor) list.

The server will ignore the message if the client does not have the permission
for a specific target.
By default, players can send only `DATA` and `PLAYER_UPDATE` messages.
