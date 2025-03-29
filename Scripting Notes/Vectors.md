## Raycast from Weapon

```sqf
onEachFrame {
    private _startPosASL = player modelToWorldWorld (player selectionPosition "righthand");
    private _endPosASL = (_startPosASL vectorAdd (player weaponDirection currentWeapon player vectorMultiply 5));
    drawLine3D [ASLToAGL _startPosASL, ASLToAGL _endPosASL, [1, 0, 0, 1]];
};
```