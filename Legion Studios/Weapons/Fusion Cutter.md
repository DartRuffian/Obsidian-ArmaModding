#core #weapons
## Overview
Each feature would be tied to its own magazine. Most units would only use one feature per role, so this saved time for players switching between different firing modes / muzzles.

Magazines will need to be quite heavy to balance out their features.
## Locking / Unlocking Doors
Shooting a door will either start to (un)lock the door depending on the state. Potentially doors could have a "health" value that would allow certain doors to be harder to slice than others.

Ideally, this would work for doors that a part of an object, e.g. a house, as well as separate door objects, e.g. Legion props.

See how ACE gets the door the player is looking at: [ACE3/addons/interaction/functions/fnc_getDoor.sqf](https://github.com/acemod/ACE3/blob/master/addons/interaction/functions/fnc_getDoor.sqf)
## Fortifications
TBD
## Repairing
Players will be able to repair vehicles by shooting a specific component with the fusion cutter. The fusion cutter would be limited to field repairs. E.g. a player would only be able to repair a vehicle component up to an "orange" level of damage, but being near a repair vehicle (vanilla / ACE) would allow the player to repair it fully.

Note that the `onHit` event will only trigger if a bullet actually penetrates the vehicle's armor. Meaning the ammo used will need to have a high penetration but low damage value.
## Slicing
Slicing a vehicle a la Battlefront 2 (2005). To slice a vehicle, the player will need to stay close to the vehicle and "shoot" at it. A variable will increase each time the vehicle is hit. Once the variable reaches 100, the crew of the vehicle will be forced out. In Battlefront 2, this variable also decreases after some time, perhaps remove X from the vehicle's "slice counter" after Y seconds of not being sliced.

AI units need to be unassigned from the vehicle before being moved out, otherwise they will try to enter the vehicle again. See [AimForTheBushes/addons/staticline/functions/fnc_jump.sqf](https://github.com/DartsArmaMods/AimForTheBushes/blob/main/addons/staticline/functions/fnc_jump.sqf#L31-L33).