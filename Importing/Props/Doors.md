How to implement doors on objects the vanilla way. The vanilla way should almost always be used because it gives mission makers an easy way to manipulate doors through the object's properties in Eden and allows other mods to control doors by (un)locking them in a standardized way.

This page currently only covers doors without handles, as they're the only type I've implemented in-game so far.

## Config

`BIS_fnc_door` handles the opening and closing of doors.
### Animations
These are the exact animation source names that `BIS_fnc_door` will animate, with `NUMBER` being the door's index (starting from 1).

The `sound` and `noSound` animation sources will always be used when opening / closing the door, and the `locked` animation will be used when trying to open a locked door.

```cpp
class AnimationSources {
    class Door_NUMBER_sound_source {
        source = "user";
        initPhase = 0;
        animPeriod = 1;
        sound = "GateDoorsSound"; // class in CfgAnimationSourceSounds
        soundPosition = "door_action_point"; // memory point for sound origin
    };
    class Door_NUMBER_noSound_source {
        source = "user";
        initPhase = 0;
        animPeriod = 1;
    };
    class Door_NUMBER_locked_source {
        source = "user";
        initPhase = 0;
        animPeriod = 0.8;
    };
};
```

### UserActions
The open / close actions still needed to be added manually via the UserActions class.

```cpp
// These allow AI to use the UserActions to open/close the doors. These point to named selections in the Path lod.
// The begin and end points should be on the opposite sides of the door.
actionBegin1 = "OpenDoor_1";
actionEnd1 = "OpenDoor_1";
actionBegin2 = "OpenDoor_2";
actionEnd2 = "OpenDoor_2";
// etc.

class UserActions {
    class OpenDoor_NUMBER {
        displayNameDefault = "<img image='\A3\Ui_f\data\IGUI\Cfg\Actions\open_door_ca.paa' size='2.5' />";
        displayName = "$STR_DN_OUT_O_DOOR";
        textToolTip = "$STR_DN_OUT_O_DOOR";
        position = "door_action_point";
        radius = 4;
        aiMaxRange = 5.25;
        priority = 100;
        onlyForPlayer = 0;
        condition = "((this animationSourcePhase 'Door_NUMBER_sound_source') < 0.03) && (cameraOn isKindOf 'CAManBase') && (alive this)";
        statement = "[this, NUMBER, 1] call BIS_fnc_door"; // [object, doorIndex, state] → state: 0=closed, 1=open
        animPeriod = 1;
    };

    class CloseDoor_NUMBER: OpenDoor_NUMBER {
        displayName = "$STR_DN_OUT_C_DOOR";
        textToolTip = "$STR_DN_OUT_C_DOOR";
        condition = "((this animationSourcePhase 'Door_1_sound_source') > 0.99) && ((this getVariable ['bis_disabled_Door_NUMBER', 0]) != 1) && (cameraOn isKindOf 'CAManBase') && (alive this)";
        statement = "[this, NUMBER, 0] call BIS_fnc_door";
    };
};
```

## Other
A few other related config properties for doors.

```cpp
// Anount of doors the object has. Checked by other scripts to get the number of doors, e.g. the Eden property that lets user open/close/lock specific doors
numberOfDoors = 1;
```

## model.cfg
Here the animations can be set up however, like a rotation for a "normal" door or a translation. Since `BIS_fnc_door` uses `animateSource`, the animation names in the model.cfg aren't relevant, as long as they are set to the correct `AnimationSources` class.

### Rotation
```cpp
class Animations {
    class door_left_rotate {
        type = "rotation";
        source = "Door_1_sound_source"; // AnimationSources class name
        selection = "door_left";
        axis = "door_left_axis";
        minValue = 0;
        maxValue = 1;
        angle0 = rad -95;
        angle1 = 0;
    };
    class door_left_locked_rot: door_left_rotate {
        source = "door_1_locked_source";
        sourceAddress = "mirror";
        minValue = 0.1;
        maxValue = 0.2;
        minPhase = 0.05;
        maxPhase = 0.2;
        angle0 = rad 0.5;
        angle1 = rad 0;
    };
};
```

### Translation
```cpp
class Animations {
    class open_door {
        type = "translation";
        source = "Door_1_sound_source";
        sourceAddress = "clamp";
        selection = "door1";
        axis = "door1_axis";
        memory = 1;
        minValue = 0;
        maxValue = 1;
        offset0 = 0;
        offset1 = 1;
    };
};
```

# Macros & Functions
For convenience, you may want to set up macros and functions to make setting up doors easier. These were developed for [Legion Studios: Battlefields](https://steamcommunity.com/sharedfiles/filedetails/?id=2320596778). If you are a member of Legion Studios' dev team, you can see them on the Battlefields GitHub repository.

[Macros](https://github.com/Legion-Studios/Battlefields/tree/main/addons/main/script_macros.hpp)
[UserActions](https://github.com/Legion-Studios/Battlefields/tree/main/addons/common/UserActions.hpp)
[fnc_doorCondition.sqf](https://github.com/Legion-Studios/Battlefields/tree/main/addons/common/functions/fnc_doorCondition.sqf)
[fnc_doorStatement.sqf](https://github.com/Legion-Studios/Battlefields/tree/main/addons/common/functions/fnc_doorStatement.sqf)