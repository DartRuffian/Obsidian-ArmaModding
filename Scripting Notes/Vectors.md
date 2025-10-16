## Raycast from Weapon

```sqf
private _startPosASL = player modelToWorldWorld (player selectionPosition "righthand");
private _endPosASL = (_startPosASL vectorAdd (player weaponDirection _weapon vectorMultiply _range));
private _object = (lineIntersectsWith [_startPosASL, _endPosASL, player, objNull, true]);

#ifdef DEBUG_MODE_FULL
DRT_test_startPosASL = ASLToAGL _startPosASL;
DRT_test_endPosASL = ASLToAGL _endPosASL;
onEachFrame {
    drawLine3D [DRT_test_startPosASL, DRT_test_endPosASL, [1, 0, 0, 1]];
};
#endif

if (_object isEqualTo []) exitWith {
    #ifdef DEBUG_MODE_FULL
    hintSilent "No Object";
    #endif
};
_object = _object select -1;

#ifdef DEBUG_MODE_FULL
hintSilent format ["Object: '%1'", typeOf _object];
#endif
```