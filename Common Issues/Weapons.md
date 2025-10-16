## My launcher magazines aren't appearing in crates, vehicles, etc. but work fine from the arsenal
Do you use the magazine for a disposable launcher with CBA? CBA will replace magazines for disposable launchers with pre-loaded launchers. You will need separate magazine classes for the normal and disposable launchers.

See [fnc_replaceMagazineCargo](https://github.com/CBATeam/CBA_A3/blob/master/addons/disposable/fnc_replaceMagazineCargo.sqf)


## Weapon `picture`s
There are several common issues when trying to set the `picture` of an item. First off, the pad *must* begin with a leading backslash (`\`).

The other common, but more complex, issue is when a weapon is configured to use the old system for pictures where you'd create separate icons for the weapon on its own, the weapon with an optic attached, the weapon with an optic and a bipod attached, etc. etc.

To fix the issue, `iconScale` in each `xSlot` class needs to be set to a value greater than 0.
```cpp
class WeaponSlotsInfo: WeaponSlotsInfo {
    class CowsSlot: CowsSlot {
        iconScale = 0.2; // Forces Arma to use the "new" layered system
    };
    class MuzzleSlot: MuzzleSlot {
        iconScale = 0.2;
    };
    class PointerSlot: PointerSlot {
        iconScale = 0.2;
    };
    class UnderBarrelSlot: UnderBarrelSlot {
        iconScale = 0.2;
    };
};
```

## Weapons with Attachments
Weapons can be configured to have certain attachments by default with its `LinkedItems` class.

```cpp
class LinkedItems {
    class LinkedItemsOptic {
        slot = "CowsSlot";
        item = "optic_Arco_AK_lush_F";
    };
    class LinkedItemsAcc {
        slot = "PointerSlot";
        item = "acc_pointer_IR";
    };
    class LinkedItemsMuzzle {
        slot = "MuzzleSlot";
        item = "muzzle_snds_B_lush_F";
    };
    class LinkedItemsUnder {
        slot = "UnderBarrelSlot";
        item = "bipod_03_F_blk";
    };
};
```

### Case-Sensitive `slot`
**The value for `slot` is case-sensitive**, and must match how it is defined in the weapon's `WeaponSlotsInfo` classes. To save yourself the headache, you should always capitalize the first letter in the name, and the s in "Slot".

Example:
```cpp
class WeaponSlotsInfo: WeaponSlotsInfo {
    class cowsslot: CowsSlot { ... };
};
class LinkedItems {
    class LinkedItemsOptic {
        slot = "CowsSlot"; // Case doesn't match WeaponSlotsInfo, won't work
        item = "optic_Arco_AK_lush_F";
    }
};
```

### Empty Classes
If you're not adding an attachment to a specific slot, don't define a class for that slot at all. Defining classes for slots with empty data can seemingly cause attachments to not be added correctly.
```cpp
// Wrong, can cause issues
class LinkedItemsOptic {
    slot = "CowsSlot";
    item = "";
};
```
