#fusion-cutter #features 

Slicing a vehicle a la Battlefront 2 (2005). To slice a vehicle, the player will need to stay close to the vehicle and "shoot" at it. A variable will increase each time the vehicle is hit. Once the variable reaches 100, the crew of the vehicle will be forced out. In Battlefront 2, this variable also decreases after some time, perhaps remove X from the vehicle's "slice counter" after Y seconds of not being sliced.

AI units need to be unassigned from the vehicle before being moved out, otherwise they will try to enter the vehicle again. See [AimForTheBushes/addons/staticline/functions/fnc_jump.sqf](https://github.com/DartsArmaMods/AimForTheBushes/blob/main/addons/staticline/functions/fnc_jump.sqf#L31-L33).