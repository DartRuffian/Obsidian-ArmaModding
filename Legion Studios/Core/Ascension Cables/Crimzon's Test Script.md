```sqf
params [
    ["_unit", objNull, [objNull]],
    ["_weapon", "", [""]],
    ["_ammo", "", [""]],
    ["_projectile", objNull, [objNull]]
];

GetMatrixFromDirAndUp = {
    params ["_vectorDir", "_vectorUp"];
  
    private _xAxis = vectorNormalized (_vectorDir vectorCrossProduct _vectorUp);
 
    [
        [_xAxis#0, _vectorDir#0, _vectorUp#0],
	    [_xAxis#1, _vectorDir#1, _vectorUp#1],
	    [_xAxis#2, _vectorDir#2, _vectorUp#2]
	];
};

fnc_delete_object = {
    params ["_playerCone"];
    sleep 15;
    deleteVehicle _playerCone;
};

_weaponClass = primaryWeapon ls_player;

private _playerPos = getPosWorldVisual ls_player;  
private _gunOffset = ls_player selectionPosition ["proxy:\a3\characters_f\proxies\weapon.001", 1];  
private _gunDir = (ls_player selectionVectorDirAndUp ["proxy:\a3\characters_f\proxies\weapon.001", 1]);  


private _model = getText (configFile >> "CfgWeapons" >> _weaponClass >> "model");
private _muzzleDirMem = getText (configFile >> "CfgWeapons" >> _weaponClass >> "muzzleEnd");
private _muzzlePosMem = getText (configFile >> "CfgWeapons" >> _weaponClass >> "muzzlePos");
private _tempObject = createSimpleObject [_model, [0,0,0], true];
private _posOffset = _tempObject selectionPosition [_muzzlePosMem, "Memory"];
private _dir = _tempObject selectionVectorDirAndUp [_muzzleDirMem, "Memory"];
private _rotMatrix = (_gunDir call GetMatrixFromDirAndUp) matrixMultiply (_dir call GetMatrixFromDirAndUp);
systemChat str _rotMatrix;
private _rotMatOffset = (_gunDir call GetMatrixFromDirAndUp) matrixMultiply matrixTranspose [_posOffset];
_rotMatOffset = (matrixTranspose _rotMatOffset) select 0;
systemChat str _rotMatOffset;
private _gunRotMatrix = _gunDir call GetMatrixFromDirAndUp;
//systemChat str _gunRotMatrix;


deleteVehicle _tempObject;
//_myCone = createVehicle ["Land_Camping_Light_F", ls_player];
_playerCone = createVehicle ["Land_Camping_Light_F", ls_player];
//hideObject _playerCone;
_myRope = ropeCreate [connectMe, [0,0,0], _playerCone, [0,0.01,0], 10];
_playerCone attachTo [ls_player, _rotMatOffset, "proxy:\a3\characters_f\proxies\weapon.001", true];

ropeUnwind [_myRope, 0.5, 1];

_playerCone spawn fnc_delete_object;
```