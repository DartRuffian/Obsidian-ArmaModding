## Inverted turret controls
If a turret's controls are inverted, check the `MainTurret` / `MainGun` animations in the vehicle's model.cfg. Depending on how the memory points are aligned, you may need to invert `angle0` and `angle1`.

```cpp
class MainTurret {
    type = "rotation";
    source = "mainTurret";
    selection = "turret";
    axis = "turret_axis";
    minValue = rad -360;
    maxValue = rad 360;
    angle0 = rad 360;
    angle1 = rad -360;
    memory = 1;
};
class MainGun: MainTurret {
    type = "rotationX";
    source = "mainGun";
    selection = "gun";
    axis = "gun_axis";
    angle0 = rad -360; // Different alignment, invert the values
    angle1 = rad 360;
};
```