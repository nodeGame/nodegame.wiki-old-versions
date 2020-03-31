- status: complete
- version: 3.x

## Top New features in nodeGame 3.x 

1. **Enhanced security**: fixed security issue that allowed clients to
connect by-passing authorization rules.

2. **Amazon Mechanical Turk integration**: command line interactive
utility for: (i) controlling the state of a HIT, and (ii) for handling
the approval/rejection of a task, granting bonuses, and assigning
qualifications.

3. **Improved resource caching**: caching enabled by default on all
files in `public/`.

4. **Improved Monitor**: improved support for dispatching games from
the interface. Results saved in the `data/` folder are automatically
available for download. Server logs are also available for download.

5. **Improved waiting room**: new options and methods to dispatch
games (e.g. player grouping and sorting).

6. **Improved requirements checkings**: new options and methods to
check requirements of incoming connections (e.g. browser detect,
resolution checkings).

6. **Improved authorization settings**: new options and methods to
authorize players (e.g. claiming id).

7. **Default channel**: a game can now be set to be the "default
game," and it will be served directly from the main server url.

8. **Improved disconnection/reconnection cycle**: proper cleanup of
previous game upon reconnection; avoiding infinite reconnection loops.

9. **Several minor bug-fixes and improvements**: in all nodeGame's
components.

## Update: v.3.5

Version 3.5 introduces **automatic matching** of players in pairs, and
automatic assignments of roles to matches. Roles can then trigger
different step properties on each client. For more info refer to the
[Matching page](Matching-Roles-Partners-v3).

## Next Topics

Next: [Migrating to v.3](Migrating-To-v3)
