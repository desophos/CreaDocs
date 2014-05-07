.. _inventory:

Inventory and Toolbar
=====================

Overview
--------

The player's inventory in Crea is handled through bags. A bag is an item which, when equipped, provides additional inventory for the player. Once equipped, a bag can only be unequipped if it is empty. The number of bags a player can equip is increased by leveling up in the gathering proficiency. Bags can also be upgraded to hold more items. Since a bag is an item that means it can be traded to other players or given to new characters.

Bags are for storing items and the toolbar is for using items. The toolbar is only a reference to an item type. Consequently, a dirt block on your toolbar would represent all of the dirt in your bags. When an item is completely consumed the item remains on the toolbar unavailable until you get more.

Modding
-------

There are two components specifically for these, InventoryComponent and ToolbarComponent. The InventoryComponent allows you to get access to all of the bags the player currently has equipped as well as check and change the items in each bag. Similarly with the ToolbarComponent you can check and change the item references in each spot.
