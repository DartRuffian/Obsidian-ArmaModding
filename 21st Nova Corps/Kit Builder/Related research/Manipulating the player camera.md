---
tags:
  - kitbuilder
  - relatedresearch
---
# Creating and entering the camera
You can create and enter the camera using the [camCreate](https://community.bistudio.com/wiki/camCreate) function in conjunction with [cameraEffect](https://community.bistudio.com/wiki/cameraEffect) function. This camera can be manipulated similar to objects using functions like [attachTo](https://community.bistudio.com/wiki/attachTo).

Example of creating the camera:
```
_camera = "camera" camcreate [0,0,0];
_camera cameraEffect ["internal", "BACK"];
```

Example of destroying the camera:
```
_camera cameraEffect ["terminate","back"];
camDestroy _camera;
```
# Camera controls
## Zoom
The focal length of the camera can be edited using the [camPrepareFov](https://community.bistudio.com/wiki/camPrepareFov) function.
The default zoom level is 0.75, with 0.01 being the narrowest and 2 being the widest focal lengths.

Example:
```
_camera camPrepareFOV 0.750;
```
## Cinematic borders
Horizontal cinematic borders can be enabled using the [showCinemaBorder](https://community.bistudio.com/wiki/showCinemaBorder) function. Where true is with cinematic borders and false is without.

It appears that cinematic borders are enabled by default. To chose to disable them, the function should be run after the camera is created.

Example:
```
showcinemaborder false;
```
## Allowing player input
Player input can be allowed using [camCommand](https://community.bistudio.com/wiki/camCommand). By default, the player is able to pan the camera but not rotate. More research needs to be done on this.

Example:
```
_camera camCommand "manual on";
```



