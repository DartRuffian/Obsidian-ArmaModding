#mortar-ai #statemachine

**Safe** = Not in "COMBAT" mode and no known enemies within 100m.
**Engaged / Combat** = In "COMBAT" mode or known enemies within 100m.
## Idle
Default state, transition to other states.

## Assemble Mortar / Get In
1. Assemble mortar
2. Mount mortar
3. Assign mortar as LAMBS artillery

If engaged, the gunner will exit and disassemble the mortar and then return fire.

## Await Fire Mission
No special behavior, waiting for a LAMBS AI to call for artillery.
If engaged, the gunner will exit and disassemble the mortar and then return fire. It is **possible** for either the gunner or other AI to spot an enemy >100m away and the gunner to then engage the enemy with the mortar. I don't have a recording of this but I swear it happened.

## Execute Fire Mission
No special behavior, LAMBS handles the ranging and main salvo shots.
If engaged, gunner will prioritize completing the fire mission and will not exit or disassemble the mortar.
**Note:** We could potentially change this for droid/organic units, where droids will prioritize the fire mission and organics will instead exit and return fire. It could make a small but cool distinction if nothing else.

## Completed Fire Mission
No special behavior, waiting for LAMBS to mark the fire mission as complete. From testing, this usually only happens after the main salvo rounds have landed.
If engaged, the gunner will exit and disassemble the mortar and then return fire.

## Disassemble Mortar / Exit
1. Mortar is removed from LAMBS artillery pool
2. Dismount mortar
3. Disassemble mortar

If safe, the mortar gunner will move to reposition, or will return fire if engaged.

## Combat
No special behavior, standard vanilla / LAMBS handling.

## Reposition
1. Move at least 500m away from last position
2. While repositioning, only engage if fired upon

The mortar gunner will try to find a position that is 500m away from their last mortar position **or** last combat position (if they were engaged while repositioning) **and** the closest enemy contact.
![[Reposition Example.excalidraw.svg]]

## Dead
Mortar gunner is dead, exit state machine.