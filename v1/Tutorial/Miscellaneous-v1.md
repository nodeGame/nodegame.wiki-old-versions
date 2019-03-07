- status: complete
- version: 1.x
- follows from: [Views](Views-v1)

## Overview

Here is a collection of additional settings and features that might be
useful for the game developer.

## Channel Query Parameters

Channel query parameters are passed as extra parameters along the url
of the nodeGame server. For example, if the url to join an experiment
is:

`http://myserver.com/mygame/`

then query parameters would be attached to the end as follows:

`http://myserver.com/mygame/?clientType=autoplay&startingRoom=Room1`

There are two channel parameters that can be set:

* clientType: sets the [client type](Client-Types-v1) that will be
  assigned the connecting client.
* startingRoom: sets the initial room where the client will be
  placed. If not specified, clients are placed first in the
  requirement room, and then moved into a waiting room.

### Enabled / disable channel parameters

Channel parameters from connecting client should be disabled when
clients are connecting from the web (i.e. through the Socket.Io
interface). To do so edit the file `channel/channel.settings.js` and
add a `sioQuery = false` field.


## Next Topics

Next: [Configure nodeGame for live experiments](Go-Live-v1)

