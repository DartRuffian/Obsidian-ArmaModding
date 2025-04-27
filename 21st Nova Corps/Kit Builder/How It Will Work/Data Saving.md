---
tags:
  - kitbuilder
  - howitwillwork
---
To save player kits, data will be stored in the player's profile using the [`profileNamespace`](https://community.bistudio.com/wiki/profileNamespace) function. The information within the player's profile will be binarized, preventing any edits by the end user.

Data will be recorded as a list of selections made by the player, rather than as a list of specific items. This approach ensures that saved kits can be updated to reflect any future loadout changes made within the mod.