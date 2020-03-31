- status: complete
- version: 3.x

## Migrating to nodeGame 3.0 and 3.2

nodeGame version 3.0 and 3.2 are fully backward-compatible with
version
2.x. For information about migrating from earlier versions refer to
the [Migrate to v.2](Migrating-To-v2) page.

### Update! Version 3.5

Version 3.5 introduced new features that could potentially be
backward-incompatible:

- New properties of the game object (rename accordingly)
     - `node.game.matcher`
     - `node.game.role`
     - `node.game.partner`
  
- New step properties (rename accordingly):
     - `matcher`
     - `roles`
  
- API changes:
     - `node.game.getProperty(property, notFound)`. The second
       parameter is now a value that should be returned in case the
       property is not found (before was gameStage). Replace with
       `node.game.plot.getProperty(gameStage, property, notFound)`.
       
- If you make use of the [Matcher API](Matching-Roles-Partners-v3),
  and you had an autoplay player, it might need to be adapted. See the
  [ultimatum autoplay](https://github.com/nodeGame/ultimatum/blob/master/game/client_types/autoplay.js)
  for a template.


## Next Topics

Next: [Start the Tutorial v.3](Getting-Started-v3)
