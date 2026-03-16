How to implement doors on objects the vanilla way. The vanilla way should almost always be used because it gives mission makers an easy way to manipulate doors through the object's properties in Eden and allows other mods to control doors by (un)locking them in a standardized way.

This page currently only covers doors without handles, as they're the only type I've implemented in-game so far.

## Scripting
`BIS_fnc_door` is the main function to handle the opening and closing of doors, and it takes the object, door number, and the target animation phase to open/close the door to.

For example, this will fully open the first door of the object you're looking at.
```sqf
[cursorObject, 1, 1] call BIS_fnc_door;
```

This will close the first door:
```sqf
[cursorObject, 1, 0] call BIS_fnc_door;
```

Doors can also be locked, by setting the `BIS_disabled_door_NUMBER` variable to `1` (`0` = unlocked). This will not hide the "Open Door" action, but will instead play the door's locked animation instead of opening the door; though the "Close Door" action will be hidden.
## Config

### UserActions
The open / close actions still needed to be added manually via the `UserActions` class. For AI to open the doors, the object will need a correctly configured PATHS lod with named selections (`actionBegin1`, `actionEnd1`, `actionBegin2`, etc.) positioned where AI should open the door from.

```cpp
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
        position = "door_NUMBER_trigger";
        radius = 4;
        aiMaxRange = 5.25;
        priority = 100;
        onlyForPlayer = 0;
        condition = "((this animationSourcePhase 'Door_NUMBER_sound_source') < 0.03) && (cameraOn isKindOf 'CAManBase') && (alive this)";
        statement = "[this, NUMBER, 1] call BIS_fnc_door";
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

### Sounds
Sounds for your door opening / closing are configured through `CfgAnimationSourceSounds`.
```cpp
class CfgAnimationSourceSounds {
    class YourPrefix_door {
        class Open {
            loop = 0;
            terminate = 0;
            trigger = "direction";
            sound0[] = {"\path\to\sound.wss", 0.4, 1};
            sound[] = {"sound0", 1};
        };
        class Close: Open {
            trigger = "1 - direction";
            sound0[] = {"\path\to\sound.wss", 0.4, 1};
        };
    };
};
```

### Animation Sources
These are the exact animation source names that `BIS_fnc_door` will animate, with `NUMBER` being the door's number (starting from 1).

The `sound` and `noSound` animation sources will always be used when opening / closing the door, and the `locked` animation will be used when trying to open a locked door. 

If there are multiple parts of a door (e.g. double doors), then there should be a selection for each part of the door, and should be named as `door_NUMBERa`, `door_NUMBERb`, etc. For example: `door_1a`, `door_1b`, `door_1c`. For door purposes, there shouldn't be a separate selection that contains all parts of the door, just the individual lettered selections. 

```cpp
class AnimationSources {
    class Door_NUMBER_sound_source {
        source = "user";
        initPhase = 0;
        animPeriod = 1;
        sound = "YourPrefix_door"; // class in CfgAnimationSourceSounds
        soundPosition = "door_NUMBER_trigger"; // memory point for sound origin (usually action position)
    };
    class Door_NUMBER_noSound_source {
        source = "user";
        initPhase = 0;
        animPeriod = 1;
    };
    class Door_NUMBER_locked_source {
        source = "user";
        initPhase = 0;
        animPeriod = 1;
    };
};
```

## Other
Other miscellaneous door config properties.

```cpp
// Anount of doors the object has. Checked by other scripts to get the number of doors, e.g. the Eden property that lets user open/close/lock specific doors
numberOfDoors = 1;
```

## model.cfg
Here the animations can be set up however, like a rotation for a "normal" door or a translation. Since `BIS_fnc_door` uses `animateSource`, the animation names in the `model.cfg` aren't relevant, as long as they are set to the correct `AnimationSources` class.

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
        source = "Door_1_locked_source";
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
        selection = "door_1";
        axis = "door_1_axis";
        memory = 1;
        minValue = 0;
        maxValue = 1;
        offset0 = 0;
        offset1 = 1;
    };
};
```

### Multi-part door
For doors that have multiple door sections / animations for a single openable door (e.g. sci-fi doors that slide in two directions or even just double doors), each part of the door should be numbered with the door's number and then lettered starting with `a`.
```cpp
class Animations {
    class open_door_1a {
        type = "translation";
        source = "Door_1_sound_source";
        sourceAddress = "clamp";
        selection = "door_1a";
        axis = "door_1a_axis";
        memory = 1;
        minValue = 0;
        maxValue = 1;
        offset0 = 0;
        offset1 = 1;
    };
    class open_door_1b: open_door_1a {
        selection = "door_1b";
        axis = "door_1b_axis";
    };
};
```

## Model
The exact setup will vary depending on how you want to animate your door, so this will just cover the basic requirements.
### Selections
Each door should have its own selection, named `door_NUMBER`. For doors that have multiple moving parts (e.g. two doors tied to a single action), they should be lettered as well beginning with `a`, e.g. `door_1a`, `door_1b`, etc.
### Memory Points
Each door should have a memory point for the location of the action, and should be named `door_NUMBER_trigger`. The axis that the door moves along should also be named `door_NUMBER_axis`.

# Macros & Functions
For convenience, you may want to set up macros and functions to make setting up doors easier. These were developed for [Legion Studios: Battlefields](https://steamcommunity.com/sharedfiles/filedetails/?id=2320596778). If you are a member of Legion Studios' dev team, you can see them on the Battlefields GitHub repository.

These will soon be added to the Public Legion Studios: Core github repository, so they'll be more available then.

[Macros](https://github.com/Legion-Studios/Battlefields/tree/main/addons/main/script_macros.hpp)
[UserActions](https://github.com/Legion-Studios/Battlefields/tree/main/addons/common/UserActions.hpp)
[fnc_doorCondition.sqf](https://github.com/Legion-Studios/Battlefields/tree/main/addons/common/functions/fnc_doorCondition.sqf)
[fnc_doorStatement.sqf](https://github.com/Legion-Studios/Battlefields/tree/main/addons/common/functions/fnc_doorStatement.sqf)