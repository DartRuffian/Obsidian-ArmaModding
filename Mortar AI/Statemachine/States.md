#mortar-ai #statemachine

## Idle
Default state, transition to other states.

## Assemble Mortar / Get In
1. Assemble mortar
2. Mount
3. Assign mortar as LAMBS artillery

## Await Fire Mission
No special behavior, waiting for LAMBS call.

## Execute Fire Mission
No special behavior, let LAMBS handle it.

## Completed Fire Mission
No special behavior, let LAMBS handle it.

## Disassemble Mortar / Exit
1. Dismount mortar
2. Disassemble mortar

## Combat
No special behavior, standard vanilla / LAMBS handling.

## Reposition
1. Move at least 500m away from last position
2. While repositioning, only engage if fired upon or enemies are within 100m

## Dead
Mortar gunner is dead, exit state machine.