## Duplicate HitPoint name 'name' in 'vehicle'
This error is most likely caused by non-unique HitPoints in Turrets. The base `NewTurret` class contains definitions for `HitGun` and `HitTurret` in its `HitPoints` class. So if multiple turrets on the same vehicle inherit from it (e.g. a gunner and commander seat) this error will be logged.

To fix it, simply rename the `HitPoint` classes in whatever ones have duplicates. In vanilla, `MainTurret` keeps `HitTurret` and `HitGun`, while the other seats have theirs renamed. E.g. `CommanderOptics` uses `HitComTurret` and `HitComGun`.

You may also have duplicate seats if you have `Turrets >> MainTurret >> Turrets >> CommanderOptics` and `Turrets >> CommanderOptics`, in which case you should likely remove one of them.
