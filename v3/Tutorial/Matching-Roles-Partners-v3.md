- status: complete
- version: 3.5+
- follows from: [Step Callback Functions](Step-Callback-Functions-v3)

## Overview

Sometimes, the same [client type](Client-Types-v3) in the same step
should behave differently depending on the _role_ that each player
assumes in that particular game step.

For example, in an <a
href="https://en.wikipedia.org/wiki/Ultimatum_game"
target="_blank">ultimatum game</a>, players are paired together, and
in the same step one player is making an offer (role = 'BIDDER') and
the other one is waiting to receive it (role = 'RESPONDENT').

It is possible to define roles and to automatically assign them (_only
matching in pairs is currently supported_) by adding properties to
game steps using the [stager API](Stager-API-v3), as explained in the
code examples below.


### Logic code

The logic must pick a matching algorithm and specify the roles inside
the `matcher` step property. Here, we match the players in pairs so
that each player meets his or her partner once in the role of 'BIDDER'
and once in the role of 'RESPONDENT.' If many rounds are played, the
matching is repeated in cycle.

```javascript
stager.extendStep('step1_bidder', {
    matcher: {

        // Roles are specified as array of two or three elements. They
        // are automatically assigned according to the matching algorithm.
        // If the number of players is odd, in turns, each player receives
        // a _bye_, that is third role in the array, the 'SOLO' role here.
        // If roles are not specified, only partner matching takes place.
        roles: [ 'BIDDER', 'RESPONDENT', 'SOLO' ],

        // The available matching algorithms: 'random' or 'roundrobin'.
        // Each player is matched with a partner and is assigned a role.
        match: 'roundrobin',

        // If there are more rounds in the stage than possible
        // matches between players, you must specify what to do after
        // all matches have been used. Available cycle options:
        //   - 'repeat': repeats exactly same partner and role matching
        //   - 'repeat_invert': repeats the same order of matching, but
        //         inverts the roles.
        //   - 'mirror': inverts the order of matching, but keeps
        //         the same roles.
        //   - 'mirror_invert': inverts both the order of matching
        //         and the roles.
        cycle: 'repeat_invert',

        // Additional options:

        // If the number of players is odd, it is possible to exclude
        // from the matches those who received the _bye_. Default: false
        skipBye: false,

        // Normally, a player receives the role and the id of the
        // partner. However, it is possible to avoid sending the id
        // of the partner. Default: false.
        sayPartner: false,

        // Overwrites the number of requested matches. Default:
        // one for every possible combination, but no more
        // than the total number of rounds in the game stage.
        // rounds: 2
    }
});
```

### Player code

By default, the role and the partner are automatically sent to each
client, and stored under the following locations:

```javascript
node.game.role = 'ROLE1';
node.game.partner = 'PARTNER_ID';
```

The player client type must now implement the roles in the
corresponding game step. Roles are nested inside the `roles` object,
where the name of the properties must match the names of the
roles. All the properties inside a role define/overwrite current step
properties.

```javascript
stager.extendStep('step1_bidder', {
    roles: {
        BIDDER: {
            frame: 'bidder.html',
            cb: function() {
                // Make an offer of 50 to the RESPONDENT.
                node.say('OFFER', this.partner, 50);
            }
        },
        RESPONDENT: {
            init: function() {
                node.game.offerReceived = null;
            },
            frame: 'resp.html',
            cb: function() {
               // Do different stuff.
            }
        },
        SOLO: {
            frame: 'solo.html',
            cb: function() {
                // Do stuff if I am not matched with any one,
                // for example if the number of players is odd.
            }
        }
    }
});
```

By default, the `role` and `partner` properties are valid only within
the same step. To re-use them across steps they either need to be sent
again by the server, or being "copied over" the next step explicitly,
like in the example below:

```javascript
stager.extendStep('step2_respondent', {
    // Role and partner meaning:
    //   - falsy      -> delete (default),
    //   - true       -> keep current value,
    //   - string     -> as is (must exist),
    //   - function   -> must return null or a valid role name
    role: function() { return this.role },
    partner: function() { return this.partner },
    roles: {
        RESPONDENT: {
            timeup: function() {
                node.game.resTimeup();
            },
            cb: function() {
                // Say to BIDDER the offer was rejected.
                node.say('REJECT', this.partner);
            }
        },
        BIDDER: {
            cb: function() {
                // Do stuff.
            }
        },
        SOLO: {
            cb: function() {
                // Keep doing stuff alone...
            }
        }
    }
});
```

### Accessing the Matches

The logic can access all the matches and roles in the step callback
via the method:

``` matcher.getMatches(<mod>, <round>);```

For example:

```javascript
stager.extendStep('step1_bidder', {
    matcher: {
        roles: [ 'BIDDER', 'RESPONDENT', 'SOLO' ],
        match: 'roundrobin',
        cycle: 'repeat_invert'
    },
    cb: function() {
        var matches, i;
        
        // Get matches for current round.
        matches = this.matcher.getMatches('ARRAY_ROLES_ID');
        
        // Inform each player of its own role and partner id.
        // (note: informing players of role and partner is done
        //  automatically by nodeGame, this is just an example).
        for (i = 0 ; i < matches.length ; i++) {
            node.say('YOU_ARE_BIDDER', matches[i].BIDDER, {
                partner: matches[i].RESPONDENT
            });
            node.say('YOU_ARE_RESPONDENT', matches[i].RESPONDENT, {
                partner: matches[i].BIDDER
            });
        }
    }
});
```

The method `matcher.getMatches()` can automatically format its return
values according to the value of the first input parameter:

 - **'ARRAY'** (default):
 
 ```[ [ 'id1', 'id2' ], [ 'id3', 'id4' ], ... ]```

 - **'ARRAY_ROLES'**:
 
 ```[ [ 'ROLE1', 'ROLE2' ], [ 'ROLE1', 'ROLE2' ] ]```

 - **'ARRAY\_ROLES\_ID'**:
 
 ```[ { ROLE1: 'id1', ROLE2: 'id2' }, { ROLE1: 'id3', ROLE2: 'id4' }, ... ]```

 - **'ARRAY\_ID\_ROLES'**:
 
 ```[ { id1: 'ROLE1', id2: 'ROLE2' }, { id3: 'ROLE1', id4: 'ROLE4' }, ... ]```

 - **'OBJ'**:
 
 ```{ id1: 'id2', id2: 'id1', id3: 'id4', id4: 'id3' }```

 - **'OBJ\_ROLES\_ID'**:
 
 ```{ ROLE1: [ 'id1', 'id3' ], ROLE2: [ 'id2', 'id4' ] }```

 - **'OBJ\_ID\_ROLES'**:
 
 ```{ id1: 'ROLE1', id2: 'ROLE2', id3: 'ROLE1', id4: 'ROLE2' }```

The second optional input parameter select a round different than
current to retrieve the matches.

Other useful API methods include:

* ```matcher.getRoleFor(id, <round>);```
* ```matcher.getMatchFor(id, <round>);```
* ```matcher.getIdForRole(role, <round>);```


## Sources

* [Ultimatum Game - Logic Client Type](https://github.com/nodeGame/ultimatum/blob/roles/game/client_types/logic.js)
* [Ultimatum Game - Player Client Type](https://github.com/nodeGame/ultimatum/blob/roles/game/client_types/player.js)
* [MatcherManager](https://github.com/nodeGame/nodegame-client/blob/roles/lib/matcher/MatcherManager.js)
* [Matcher](https://github.com/nodeGame/nodegame-client/blob/roles/lib/matcher/Matcher.js)
* [Roler](https://github.com/nodeGame/nodegame-client/blob/roles/lib/matcher/Roler.js)

## Next Topics

* [Have a closer look at how the player list works](PlayerList-v3) 
