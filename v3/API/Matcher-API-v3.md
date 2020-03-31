- status: complete
- version: 3.x

## Overview

Matching players is a new feature supported only in the development
version of nodeGame for the moment. The api is limited to matching
players into _pairs_.

The only matching algorithm is already implemented is _round robin_
(also known as _perfect stranger_), but custom matching algorithms can
be plugged into the api easily.

### Matcher API example

Import the Matcher class into your code.

```javascript
var Matcher = require('nodegame-client').Matcher;
var matcher = new Matcher();
```

Generate the tournament table with a given matching algorithm
(`roundrobin` is the only option for now), and pass any extra option.

```javascript
matcher.generateMatches('roundrobin', 4); 
```

Set the ids of the players. They will substitute the numbers in the
tournament table.

```javascript
matcher.setIds([ 'a', 'b', 'c', 'd' ])
```

Match the ids of the players into the round robin tournament
schedule. By default, the ids are randomly assigned, but other
assignment rules can be defined.

```javascript
matcher.match();
// The whole tournament schedule is defined as follow:
[ 
  // Round 1.
  [ ['a', 'b'], ['c', 'd'] ],
  // Round 2.
  [ ['c', 'b'], ['a', 'd'] ],
  // Round 3.
  [ ['c', 'a'], ['b', 'd'] ]
] 
```

Call getMatch to receive the next match.

```javascript
matcher.getMatch(); // Returns ['a', 'b']
matcher.getMatch(); // Returns ['c', 'd']
matcher.getMatch(); // Returns ['c', 'b']

// When all the matches are exhausted null will be returned
```

### Round Robin API advanced

#### Available options

The round robin scheduler accept two options: the number of players
and a configuration object. Available options for the configuration
objects are:

  - `bye`: _number_ (Default: -1) sets the number assigned to dummy
       players in tournament tables with odd number of participants.
  - `skypBye`: _boolean_ (Default: false) controls whether matches
       with dummy players are included in the final schedule.
  - `missingId`: _string_ (Default: 'bot') sets the id for players
    whose number in the tournament table cannot be resolved.
  - `assignerCb`: _function_ (Default: Matcher.randomAssigner) sets
    the assigner callback
  - `x`: _number_ (Default 0) sets the round index for method
    _getMatch_ when called with no parameter.
  - `y`: _number_ (Default 0) sets the match index for method
    _getMatch_ when called with no parameter.

The matcher can be re-initialized calling the _init_ method any time.

```javascript
matcher.init(options);
```


#### Get specific matches

It is possible to request specific matches by passing parameters to
the `getMatch` method.

```javascript
matcher.getMatch(1,1); // ['c', 'b']
matcher.getMatch(3,1); // null (out of bounds)
```

#### Get all round matches

It is possible to get all the matches of a round by leaving out the
second parameter to `getMatch`

```javascript
matcher.getMatch(1); // [ ['c', 'b'], ['a', 'd'] ]
```

#### Get round matches as key-value pairs

To get all the matches of a round in one object use the
`getMatchObject` method, with or without the round parameter.

```javascript
matcher.getMatchObject(); // {a: 'b', c: 'd' }
matcher.getMatchObject(1); // { c: 'b', a: 'd' }
```


#### Custom id assignment

By default, the specified ids will be randomly placed into the
tournament schedule. However, if a special assignment is required, it
is possible to specify an assigner callback

```javascript
matcher.setAssignerCb(function(ids) {
    return [ ids[1], ids[2], ids[0] ];
});
```

The assigner callback is called before performing a matching. The
order of the ids inside the array returned by the functions determines
how the players are assigned to the schedule.

## Next Topics

* [Return to home](Home)
