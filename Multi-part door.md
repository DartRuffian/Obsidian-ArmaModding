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